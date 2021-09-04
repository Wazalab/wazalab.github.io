---
id: 548
title: Crostini VMの再起動方法
date: 2018-09-18T21:03:31+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2018/09/18/547-revision-v1/
permalink: /?p=548
---
crostiniのXがよく落ちるのでCrostini VMを再起動するケースがよくある。手順は簡単で、

1. [crosh](https://chrome.google.com/webstore/detail/crosh-window/nhbmpbdladcchdhkemlojfjdknjadhmh)を起動
2. vmc stop termina
3. vms start termina

 
<pre class="lang:sh decode:true " >crosh&gt; vmc list
termina
Total Size (bytes): 4176281600
crosh&gt; vmc stop termina
crosh&gt; vmc start termina
</pre> 
