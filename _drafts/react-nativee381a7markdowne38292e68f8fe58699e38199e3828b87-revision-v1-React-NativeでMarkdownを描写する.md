---
id: 89
title: React NativeでMarkdownを描写する
date: 2016-09-17T10:15:04+09:00
author: Shohei
layout: revision
guid: http://www.wazalab.com/2016/09/17/87-revision-v1/
permalink: /?p=89
---
MarkdownのテキストをReact Native側で表示したいケースが出てきました。　リサーチすると一番最初に出てきたのが[react-native-markdown](https://github.com/lwansbrough/react-native-markdown)というモジュールです。少し触っていみると、最近のReact Nativeにキャッチアップしてないし、Markdown文法がWIP(Work In Progress)のステータスがあって実際の使用は難しそうでした。

リサーチを続けると、その次に出てきたのが[react-native-showdown](https://github.com/lwansbrough/react-native-markdown)というモジュールです。なんとWebviewを使ってMarkdownを描写してしまう代物です。これなら、いけそうな気がしたので導入を決めました。

 <pre class="lang:default decode:true " title="イントール方法" >$ npm install --save react-native-showdown</pre> 

でサクッとイントールして、ドキュメント通りに使用します。

```
import React, { Component } from 'react';
import Markdown from 'react-native-showdown';

let Task = React.createClass({
  render() {
    const { task } = this.props;
    let markdownText = `#${task.title} \n\n${task.description}`;
    return (
       <Markdown body={ markdownText } pureCSS={pureCss} style={{marginTop: 60, marginLeft: 5, marginRight: 5}} />
    );
  }
});

const pureCss = `
*{
  color: #333;
  font-family: "游ゴシック", YuGothic, "ヒラギノ角ゴ Pro", "Hiragino Kaku Gothic Pro", "メイリオ", "Meiryo", sans-serif;
}
`
export default Task;
```

ポイントはpureCSSを差し込むところですね。ES6のTemplate Stringはヒアドキュメントっぽくかけるので便利です。細かい調整はここでできるとおもいます。

Webviewを使うので、Reactっぽさはないですが描写限定で使うには非常に便利だと思います。

