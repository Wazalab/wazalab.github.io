---
id: 851
title: '[iOS] React Native Webviewでsourceの指定方法'
date: 2019-12-18T13:06:14+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2019/12/18/839-revision-v1/
permalink: /?p=851
---
[React Native communityのWebview](https://github.com/react-native-community/react-native-webview)でiOSの指定方法です。

### 1. 変数からHTMLをロードする
### 2. URIからrequire経由

 
<pre class="lang:default decode:true " >    &lt;WebView
        ref={r =&gt; (this.webref = r as any)}
        originWhitelist={["*"]}
        source={require("../assets/search.html")}
        source={HTML}</pre> 


### 3. Xcodeのアセットを利用してのロード

RNFSを利用しファイルを読み込みます。
<pre class="decode:true">import RNFS from "react-native-fs";
... 
  &lt;WebView
        originWhitelist={["*"]}
        source={{ uri: `file://${RNFS.MainBundlePath}/search.html` }}
</pre>

##### 利点
 * SafariでSimulator経由でデバッグできる
 * HTMLで`<script src="./input.js">`できる。srcは、同じ階層に置けば良い。
##### 欠点
 * XCodeでのRebuildが必要になる 