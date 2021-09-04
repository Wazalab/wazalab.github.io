---
id: 184
title: '[React Native] Androidのパッケージ名の変更方法'
date: 2017-04-29T11:07:14+09:00
author: Shohei
layout: revision
guid: http://www.wazalab.com/2017/04/29/174-revision-v1/
permalink: /?p=184
---
react-nativeコマンドで、アプリを作ると`kagami.com`という名前空間を自動生成するが、これを`kagami.wazalab.com`というようなXcodeのようにもう一段階ネストしたい。
下記の3ステップを踏めばよい。

#### 1. ディレクトリ変更
<pre class="theme:dark-terminal lang:default decode:true ">$ mkdir android/app/src/main/java/com/wazalab
$ cd  android/app/src/main/java/com
$ mv kagami wazalab</pre>
#### 2. ファイル内のパッケージ名変更

`com.kagami`から`com.wazalab.kagami`に変更する。
<ul>
 	<li>android/app/src/main/java/com/wazalab/kagami/MainActivity.java</li>
 	<li>android/app/src/main/AndroidManifest.xml</li>
 	<li>android/app/build.gradle</li>
</ul>
#### 3. 再生成
<pre class="theme:dark-terminal lang:default decode:true "> $ cd android
 $ ./gradlew clean
 $ cd ..
 $ react-native run-android
</pre>
起動しようとすると、下のようなエラーがでる。deeplinkが動かないみたなことがあるらしいが、これは基本、自動でアプリが起動しないだけ。エミュレータにいってタップして起動させる。
(githubに[issue](https://github.com/facebook/react-native/issues/5546#ref-pullrequest-166889844)が上がっているが、PRがマージされてない様子...)
```
Error type 3
Error: Activity class {com.wazalab.kagami/com.wazalab.kagami.MainActivity} does not exist.
```