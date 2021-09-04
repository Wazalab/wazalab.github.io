---
id: 767
title: macOSでkey repeatが動かない時の対処法
date: 2019-07-10T08:51:39+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2019/07/10/766-revision-v1/
permalink: /?p=767
---
実際はkeyrepeatは作動している。（例えば、$などはブラウザ上でキーリピートするし、Terminalでは問題なく動く)
この現象のときは、下記をターミナルを開いて打つ。

<pre class="theme:dark-terminal lang:default decode:true " >defaults write -g ApplePressAndHoldEnabled -bool false</pre> 

そして、再起動。