---
id: 839
title: '[iOS] React Native Webviewでsourceの指定方法'
date: 2019-12-18T13:06:22+09:00
author: Shohei
layout: post
guid: https://www.wazalab.com/?p=839
permalink: /?p=839
siteorigin_page_settings:
  - 'a:6:{s:6:"layout";s:7:"default";s:10:"page_title";b:1;s:15:"masthead_margin";b:1;s:13:"footer_margin";b:1;s:16:"display_masthead";b:1;s:22:"display_footer_widgets";b:1;}'
categories:
  - IT
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