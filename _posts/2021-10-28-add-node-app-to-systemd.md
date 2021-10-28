---
title: Node app(redashbot)をsystemdで起動する方法 
date: 2021-10-28
author: Shohei
layout: post
parmalink: /2021/10/28/add-node-app-to-systemd
siteorigin_page_settings:
  - 'a:6:{s:6:"layout";s:7:"default";s:10:"page_title";b:1;s:15:"masthead_margin";b:1;s:13:"footer_margin";b:1;s:16:"display_masthead";b:1;s:22:"display_footer_widgets";b:1;}'
categories:
  - IT
tags:
  - systemd 
  - node 
--- 

Redash botが使用できなくなったので、代替を探していたら[redashbot](https://github.com/hakobera/redashbot)を見つけて早速導入。
(Redashは買収後元気ありませんね。。)

導入後Rebootに耐えたいよね、ってことでsystemdに登録してみた。以下手順。


### serviceファイル作成 

```
 $ sudo vi /etc/systemd/system/redashbot.service
```

中身は、

```
[Unit]
Description=Redashbot

[Service]
PIDFile=/tmp/redashbot.pid
Restart=always
KillSignal=SIGQUIT
WorkingDirectory=/home/ubuntu/redashbot
EnvironmentFile=/home/ubuntu/redashbot/bot.env
ExecStart=npm start
User=ubuntu
Group=ubuntu

[Install]
WantedBy=multi-user.target
```

bot.envは、

```
SLACK_BOT_TOKEN=xxxx
REDASH_HOST=yyy
SLACK_BOT_TOKEN=
```

### systemdにセットしてスタート

```
 $ sudo systemctl daemon-reload
 $ sudo systemctl start redashbot
```

### Reboot後も起動するようにする 

```
 $ sudo systemctl enable redashbot
```



