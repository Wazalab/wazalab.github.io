---
id: 159
title: '[React Native] AnalyzerでBUILD FAILEDな時の対処法'
date: 2017-04-28T10:38:03+09:00
author: Shohei
layout: revision
guid: http://www.wazalab.com/2017/04/28/156-revision-v1/
permalink: /?p=159
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
