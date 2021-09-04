---
id: 658
title: '[Linux] rbenv listの更新方法'
date: 2019-01-08T07:21:56+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2019/01/08/655-autosave-v1/
permalink: /?p=658
---
毎年クリスマスにrubyがでるので更新する。Linuxにbrewはないのでgitでpullする。

<pre class="theme:dark-terminal lang:default decode:true " >cd "$(rbenv root)"/plugins/ruby-build &amp;&amp; git </pre>