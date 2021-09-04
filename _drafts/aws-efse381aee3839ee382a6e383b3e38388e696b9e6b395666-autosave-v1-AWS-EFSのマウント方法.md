---
id: 675
title: '[AWS] EFSのマウント方法'
date: 2019-01-14T14:28:53+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2019/01/14/666-autosave-v1/
permalink: /?p=675
---
EFSのルートを/etc/letsencryptをマウントする方法は、

<h3>1. パッケージのインストール (Ubuntuの場合)</h3>

<pre class="theme:dark-terminal lang:default decode:true " ># apt-get -y install nfs-common</pre>

<h3>2. 下記を/etc/fstabに記述する</h3>

<pre class="theme:dark-terminal lang:default decode:true " >YOUR_EFS.efs.ap-northeast-1.amazonaws.com:/ /etc/letsencrypt nfs4 nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,_netdev 0 0</pre>

<h3>3. セキュリティグループの作成</h3>

<h4>EFSサイドは2049 inboundをオープン</h4>

<img src="https://www.wazalab.com/wp-content/uploads/2019/01/スクリーンショット-2019-01-14-14.26.44-1024x125.png" alt="" width="1024" height="125" class="alignnone size-large wp-image-674" />

<h3>4. 該当ディレクトリを作成し、fstabのReload</h3>

<pre class="theme:dark-terminal lang:default decode:true " ># mkdir /etc/letsencrpyt 
# mount -a </pre>