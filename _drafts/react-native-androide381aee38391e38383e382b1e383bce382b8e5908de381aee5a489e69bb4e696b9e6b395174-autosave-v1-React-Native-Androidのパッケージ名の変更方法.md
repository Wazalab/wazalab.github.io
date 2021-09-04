---
id: 182
title: '[React Native] Androidのパッケージ名の変更方法'
date: 2017-04-29T11:05:48+09:00
author: Shohei
layout: revision
guid: http://www.wazalab.com/2017/04/29/174-autosave-v1/
permalink: /?p=182
---
react-nativeコマンドで、アプリを作ると<code>kagami.com</code>という名前空間を自動生成するが、これを<code>kagami.wazalab.com</code>というようなXcodeのようにもう一段階ネストしたい。
下記の3ステップを踏めばよい。

<h4>1. ディレクトリ変更</h4>

<pre class="theme:dark-terminal lang:default decode:true " >$ mkdir android/app/src/main/java/com/wazalab
$ cd  android/app/src/main/java/com
$ mv kagami wazalab</pre>

<h4>2. ファイル内のパッケージ名変更</h4>

<code>com.kagami</code>から<code>com.wazalab.kagami</code>に変更する。

android/app/src/main/java/com/wazalab/kagami/MainActivity.java
ndroid/app/src/main/AndroidManifest.xml
*　android/app/build.gradle

<h4>3. 再生成</h4>

<pre class="theme:dark-terminal lang:default decode:true " >
 $ cd android
 $ ./gradlew clean
 $ cd ..
 $ react-native run-android

</pre>

起動しようとすると、下のようなエラーがでる。deeplinkが動かないみたなことがあるらしいが、これは基本、自動でアプリが起動しないだけ。エミュレータにいってタップして起動させる。
(githubに<a href="https://github.com/facebook/react-native/issues/5546#ref-pullrequest-166889844">issue</a>が上がっているが、PRがマージされてない様子...)

<pre><code>Error type 3
Error: Activity class {com.wazalab.kagami/com.wazalab.kagami.MainActivity} does not exist.
</code></pre>