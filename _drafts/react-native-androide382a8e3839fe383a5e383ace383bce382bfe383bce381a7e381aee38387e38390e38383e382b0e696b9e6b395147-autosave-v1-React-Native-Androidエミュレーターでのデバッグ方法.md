---
id: 172
title: '[React Native] Androidエミュレーターでのデバッグ方法'
date: 2017-04-29T10:16:25+09:00
author: Shohei
layout: revision
guid: http://www.wazalab.com/2017/04/29/147-autosave-v1/
permalink: /?p=172
---
<img class="alignnone size-medium wp-image-148" src="http://www.wazalab.com/wp-content/uploads/2017/04/スクリーンショット-2017-04-18-8.56.02-162x300.png" alt="" width="50%" />

<code>CMD-m</code>でデバッグwindow表示して、Debug JS remotelyをクリック。
でも、ものすごく遅いので、ターミナルから下記を走らせた方がいい。
（表示はChromeのデバッグほどきれいじゃないけど。。）

<pre class="theme:dark-terminal lang:sh decode:true " >$ adb logcat *:S ReactNative:V ReactNativeJS:V</pre>