---
id: 754
title: '[LXD/LXC] lxd imageのexportとimport方法'
date: 2019-06-20T09:56:25+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2019/06/20/748-revision-v1/
permalink: /?p=754
---
LXDのversionは3.03

## LXD export

作成してあるコンテナのイメージではなくて初期起動時にダウンロードしてくるOSイメージをexportするには`lxc launch`で指定するイメージ名を引数にいれてexportする。
 
<pre class="theme:dark-terminal lang:default decode:true " >$ lxc image export ubuntu:18.04 .</pre> 

## LXD import

version3.03ではtar.xzとsquashfsが作成されるのでこれを２つとも指定し、importする。aliasで名前をイメージ名を指定できる。`:`（コロン)は使えないので注意。


<pre class="theme:dark-terminal lang:default decode:true " >lxc image import ubuntu-18.04-server-cloudimg-amd64-lxd.tar.xz ubuntu-18.04-server-cloudimg-amd64.squashfs --alias ubuntu18.local</pre> 


## launchまでの速度比較

同じfingerprintのイメージをimageリストに持てないので、ubuntu16と18で比較してますが、、だいたい同じになると思います。

#### イメージダウンロード後にLaunch

 
<pre class="theme:dark-terminal lang:default decode:true " >$ time lxc launch ubuntu:16.04 default2
Creating default2
Starting default2 

real 3m32.754s
user 0m0.059s
sys 0m0.027s</pre> 


約３分半。


#### importしてlaunch

<pre class="theme:dark-terminal lang:default decode:true " >$ time lxc image import ubuntu-18.04-server-cloudimg-amd64-lxd.tar.xz ubuntu-18.04-server-cloudimg-amd64.squashfs --alias ubuntu18.local
Image imported with fingerprint: c234ecee3baaee25db84af8e3565347e948bfceb3bf7c820bb1ce95adcffeaa8

real 0m5.358s
user 0m0.171s
sys 0m0.551s

root@ip-172-31-36-58:~# time lxc launch ubuntu18.local test
Creating test
Starting test

real 0m46.986s
user 0m0.026s
sys 0m0.005s</pre> 

52秒ぐらいで２分半は早く起動できることを確認！



