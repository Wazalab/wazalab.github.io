---
id: 700
title: '[Linux] シンプルなプロセス監視にはimmortalが便利'
date: 2019-03-06T09:10:26+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2019/03/06/693-revision-v1/
permalink: /?p=700
---
プロセスがなにかの理由で死んだら、単純にリスタートさせたい。けど、systemdとかmonitとかの設定ファイルを書くのが面倒だなって思っていて調べていた。シェル(Bash)でLoopでかけそうだけど、
 
<pre class="theme:dark-terminal lang:bash decode:true " >until myserver; do
    echo "Server 'myserver' crashed with exit code $?.  Respawning.." &gt;&amp;2
    sleep 1
done</pre> 

Ref: [Stackoverflow](https://stackoverflow.com/questions/696839/how-do-i-write-a-bash-script-to-restart-a-process-if-it-dies)

これも面倒だと思ってしまいました。スクリプトの前にone command置く感じでやりたいなって調べてみると、[immortal](https://immortal.run/)というツールを発見！早速インストール。


<pre class="theme:dark-terminal lang:default decode:true " >$ curl -s https://packagecloud.io/install/repositories/immortal/immortal/script.deb.sh | sudo bash
$ sudo apt-get install immortal</pre> 


使い方もシンプルで
<pre class="theme:dark-terminal lang:default decode:true " >$ immortal /bin/sh -c "sleep 5 &amp;&amp; date &gt; /tmp/sleep.log"</pre> 


付属のimmortalctlで、管理できる。
<pre class="theme:dark-terminal lang:default decode:true " >$ immortalctl                                
  PID     Up   Down   Name    CMD                                                                                      
22400   4.6s          22364   /bin/sh -c sleep 5 &amp;&amp; date &gt; /tmp/sleep.log 
$ immortalctl halt 22364  </pre> 
 
