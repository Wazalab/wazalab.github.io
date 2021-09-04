---
id: 514
title: '[Ubuntu] 古いバージョンのElasticsearchのインストール方法'
date: 2018-09-13T20:59:20+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2018/09/13/507-revision-v1/
permalink: /?p=514
---
本家のバージョンがどんどん上がっているくが、現状問題ないならあえて追従する必要はないように思う。（セキュリティに重大な問題等なければ） 

新しい環境を設定するときに、古いバージョンをインストールするのは少し面倒臭い。

[https://www.elastic.co/jp/downloads/past-releases](https://www.elastic.co/jp/downloads/past-releases) でProductとVersionを選び。ダウンロードボタンをクリックし、該当リンクをコピペ。

<img src="https://www.wazalab.com/wp-content/uploads/2018/09/スクリーンショット-2018-09-13-20.54.48-1024x339.png" alt="" width="1024" height="339" class="alignnone size-large wp-image-512" />

 
<pre class="lang:sh decode:true " >$ curl -LO https://download.elastic.co/elasticsearch/release/org/elasticsearch/distribution/deb/elasticsearch/2.4.1/elasticsearch-2.4.1.deb
$ sudo dpkg -i elasticsearch-2.4.1.deb &amp;&amp; rm elasticsearch-2.4.1.deb</pre> 

ちなみにconfigがないと怒られるので

 
<pre class="lang:sh decode:true " >$ sudo ln -s /etc/elasticsearch/ /usr/share/elasticsearch/config</pre> 


またrootで起動できないとも言われるので

 
<pre class="lang:sh decode:true " >$ sudo /usr/share/elasticsearch/bin/elasticsearch -Des.insecure.allow.root=true</pre> 

