---
id: 639
title: '[Linux] timezone/zoneinfoの確認方法'
date: 2018-11-10T11:16:32+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2018/11/10/637-autosave-v1/
permalink: /?p=639
---
Heroku-16でブラジルのタイムゾーンが間違っててブラジルに住むユーザさんから時間がおかしいとの問い合わせがあった。
どうやって確認したかというtzdataのzoneinfoをzdumpすればタイムゾーンでの時間がわかる。

<pre class="theme:dark-terminal lang:default decode:true " >~ $ date
Sun Oct 21 22:58:21 UTC 2018
~ $ zdump /usr/share/zoneinfo/Brazil/*
/usr/share/zoneinfo/Brazil/Acre       Sun Oct 21 17:58:53 2018 -05
/usr/share/zoneinfo/Brazil/DeNoronha  Sun Oct 21 20:58:53 2018 -02
/usr/share/zoneinfo/Brazil/East       Sun Oct 21 20:58:53 2018 -02
/usr/share/zoneinfo/Brazil/West       Sun Oct 21 18:58:53 2018 -04
</pre>

で、Heroku側に問い合わせると、Heroku-18にアップグレードするかbuildpackでtzdataを上書きしろと。。
heroku16は使ってるrubyバージョンがなく動かないのをしっていたのでrubyを最新バージョン、railsを5.0001から5.07にアップグレードして解決