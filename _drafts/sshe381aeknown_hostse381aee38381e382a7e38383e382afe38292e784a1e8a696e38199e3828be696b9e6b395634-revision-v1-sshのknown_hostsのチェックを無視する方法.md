---
id: 636
title: sshのknown_hostsのチェックを無視する方法
date: 2018-11-06T08:49:41+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2018/11/06/634-revision-v1/
permalink: /?p=636
---
動的にドメインを貼るようなシステムだと毎回SSHするたびにknown_hostが違うと怒られる。これを回避するには、いくつか方法があるが`.ssh/config`に下記を書くのが一番簡単だと思う。


 
<pre class="theme:dark-terminal lang:default decode:true " >Host myhost.devany.net
  StrictHostKeyChecking no
</pre> 
