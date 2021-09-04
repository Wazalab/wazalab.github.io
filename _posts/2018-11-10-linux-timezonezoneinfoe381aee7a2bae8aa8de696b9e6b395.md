---
id: 637
title: '[Linux] timezone/zoneinfoの確認方法'
date: 2018-11-10T11:16:29+09:00
author: Shohei
layout: post
guid: https://www.wazalab.com/?p=637
permalink: '/2018/11/10/linux-timezonezoneinfo%e3%81%ae%e7%a2%ba%e8%aa%8d%e6%96%b9%e6%b3%95/'
siteorigin_page_settings:
  - 'a:6:{s:6:"layout";s:7:"default";s:10:"page_title";b:1;s:15:"masthead_margin";b:1;s:13:"footer_margin";b:1;s:16:"display_masthead";b:1;s:22:"display_footer_widgets";b:1;}'
categories:
  - Infrastructure
  - IT
tags:
  - heroku
  - timezone
  - tzdata
  - zoneoinfo
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