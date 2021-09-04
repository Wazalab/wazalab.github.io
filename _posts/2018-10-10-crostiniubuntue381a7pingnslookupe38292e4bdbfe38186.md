---
id: 622
title: Crostini(Ubuntu)でping/nslookupを使う
date: 2018-10-10T22:00:32+09:00
author: Shohei
layout: post
guid: https://www.wazalab.com/?p=622
permalink: '/2018/10/10/crostiniubuntu%e3%81%a7pingnslookup%e3%82%92%e4%bd%bf%e3%81%86/'
siteorigin_page_settings:
  - 'a:6:{s:6:"layout";s:7:"default";s:10:"page_title";b:1;s:15:"masthead_margin";b:1;s:13:"footer_margin";b:1;s:16:"display_masthead";b:1;s:22:"display_footer_widgets";b:1;}'
categories:
  - IT
tags:
  - crostini
  - nslookup
  - ping
  - ubuntu
---
Crostiniというより、Ubuntuなのですが、nslookupがなくて名前解決できなくて辛い。
hostコマンドを使うところなんだろうけど、気づけばnslookupを打ってるのでやはり欲しい。

`dnsutils`というパッケージに入っているので、これを入れるだけ。

あとpingは普通に使うでしょってことでこれもapt-getで。

 
<pre class="theme:dark-terminal lang:default decode:true " >$ sudo apt-get install dnsutils
$ sudo apt-get install iputils-ping
</pre> 
