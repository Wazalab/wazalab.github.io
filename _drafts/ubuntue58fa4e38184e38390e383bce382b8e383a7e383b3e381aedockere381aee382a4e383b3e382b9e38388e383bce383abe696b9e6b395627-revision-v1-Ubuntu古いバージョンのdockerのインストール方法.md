---
id: 628
title: '[Ubuntu]古いバージョンのdockerのインストール方法'
date: 2018-10-10T22:13:22+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2018/10/10/627-revision-v1/
permalink: /?p=628
---
ある機能が現バージョンは動かなかったりで、昔のdockerをインストールすることがあった。
パッケージレポを設定して、`apt-cache policy`でイントールできるバージョンを確認して、現行のをアンインストールして、該当パッケージを入れるだけ。


 
<pre class="theme:dark-terminal lang:default decode:true " >sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu xenial stable"
sudo apt-get update
# to show package list that can be installed
sudo apt-cache policy docker-ce
# delete current one
sudo apt-get purge docker-ce -y
sudo apt-get install -y docker-ce=17.06.2~ce-0~ubuntu</pre> 


