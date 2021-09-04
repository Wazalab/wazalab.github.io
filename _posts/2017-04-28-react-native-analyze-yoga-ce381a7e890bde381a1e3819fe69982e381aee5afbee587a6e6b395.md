---
id: 156
title: '[React Native] AnalyzerでBUILD FAILEDな時の対処法'
date: 2017-04-28T10:37:11+09:00
author: Shohei
layout: post
guid: http://www.wazalab.com/?p=156
permalink: '/2017/04/28/react-native-analyze-yoga-c%e3%81%a7%e8%90%bd%e3%81%a1%e3%81%9f%e6%99%82%e3%81%ae%e5%af%be%e5%87%a6%e6%b3%95/'
categories:
  - App
  - IT
  - Programming
tags:
  - react-native
---
Projectのディレクトリを移動されたら`react-native run-ios`で失敗するようになった。


```
** BUILD FAILED **


The following commands produced analyzer issues:
        Analyze /Users/shohey1226/git/Kagami/node_modules/react-native/ReactCommon/yoga/yoga/Yoga.c

        Analyze /Users/shohey1226/git/Kagami/node_modules/react-native/ReactCommon/yoga/yoga/YGNodeList.c
(2 commands with analyzer issues)

The following build commands failed:
        Analyze /Users/shohey1226/git/Kagami/node_modules/react-native/ReactCommon/yoga/yoga/Yoga.c
        Analyze /Users/shohey1226/git/Kagami/node_modules/react-native/ReactCommon/yoga/yoga/YGNodeList.c
(2 failures)
```

キャッシュを使用してBuildを試みてて失敗しているようなので、ビルドを消す。

<pre class="theme:dark-terminal lang:default decode:true " >
$ rm -rf ios/build
$ react-native run-ios
</pre> 
