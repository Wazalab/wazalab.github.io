---
layout: post
title: "heroku pg:diagnoseをslackに投げる方法"
date: 2022-01-20 01:34 +0000
categories:
  - IT
  - Infra
tags:  
  - heroku
  - slack
---

年末の休暇でコード変更がなかったのが原因かわからないが、herokuのpostgresが謎のout of memoryを吐いて止まりまくる現象が起きた。
しかも、成人の日の三連休中で対応が遅れてしまった。問題が当日わからなかったので、プロセスを再起動などの処理をして、1日様子見。
そして、次の日も問題がお昼ごろに起きたので、follower DBをマスタに切り替えて問題は治った。

週末にherokuに問い合わせたものが火曜日に返信がきた。問題は、CPUがバーストしてたのが原因のこと。
モニタしていなかったので、一日に何回かheroku pg:diagnoseを叩き、それをslackに送るワンライナーを書いた。

```shell
$ heroku pg:diagnose --app APP_NAME | curl -X POST --data-urlencode   "payload={\"text\": \"$(</dev/stdin)\"}" https://hooks.slack.com/services/xxxxxxxxxxx
```

cronに設定しようとしたら動かない。。仕方がないのでshellを書いたら動いた。

```
#!/bin/bash -x

out=`/snap/bin/heroku pg:diagnose --json --app APP_NAME | /usr/bin/jq -r '.checks[] | {"name": .name, "result": .results} | select(.name |test("Burst"))' | egrep 'name|balance' | sed s/\"//g 2>&1`
echo $out 
curl -X POST --data-urlencode "payload={\"text\": \"$out\"}" https://hooks.slack.com/services/T02xxxxxxxxxxxxxxx
```
