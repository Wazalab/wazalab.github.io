---
id: 576
title: '[ChromeOS] Crostiniで日本語入力する'
date: 2018-09-19T22:05:46+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2018/09/19/554-revision-v1/
permalink: /?p=576
---
ここ何年もLinuxに触ってなかったので久しぶりに日本語入力系の話。昔は、Cannaとか使ってましたね。SKK/Anthyで院生のときは過ごしてたと思います。

Crostiniでも日本語入力を扱いたいのでGoogleの日本語入力のMozc(モズク)を初インストール。

<pre class="theme:dark-terminal lang:default decode:true " >$ sudo apt-get install fcitx-mozc</pre> 

fcitxというのも初めて。apt-getでプロセスが上がってくる(確か)

 
<pre class="theme:dark-terminal lang:default decode:true " >$ ps -ef | grep fci
shohei.+   264    88  0 08:13 ?        00:00:03 fcitx
shohei.+   280    88  0 08:13 ?        00:00:01 /usr/bin/dbus-daemon --fork --print-pid 5 --print-address 7 --config-file /usr/share/fcitx/dbus/daemon.conf
shohei.+   284    88  0 08:13 ?        00:00:00 /usr/bin/fcitx-dbus-watcher unix:abstract=/tmp/dbus-jYqdpnGPBg,guid=2870007bcc26b2aa1f1477c15ba186aa 280
shohei.+  1562   621  4 08:32 pts/2    00:00:06 fcitx-config-gtk3
shohei.+  1661  1593  0 08:34 pts/4    00:00:00 grep fci</pre> 


XMODIFIERSを設定しろと言われたので下記のコマンドで環境変数を設定する。


 
<pre class="theme:dark-terminal lang:default decode:true " >＄ export XMODIFIERS=@im=fcitx</pre> 



fcitx-configtoolでGUIから設定できる。

<pre class="theme:dark-terminal lang:default decode:true " >
$ fcitx-configtool
</pre>

inputメソッドにmozcを追加。
<img src="https://www.wazalab.com/wp-content/uploads/2018/09/ea51ba6c-9f68-4226-9218-63ae3162aef3.png" alt="" width="960" height="231" class="alignnone size-full wp-image-560" />

英語キーボードなのでLayoutを指定。
<img src="https://www.wazalab.com/wp-content/uploads/2018/09/0fce1fd1-396c-4418-8cac-2e50c26e8a71.png" alt="" width="818" height="377" class="alignnone size-large wp-image-559" />

firefoxか何かをあげてctrl+spaceで入力が表示される。
