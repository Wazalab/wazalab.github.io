---
id: 260
title: Redux + immutable.js + Reselectでredux reducerを安全かつ簡潔にする
date: 2017-08-24T12:49:09+09:00
author: Shohei
layout: post
guid: http://www.wazalab.com/?p=260
permalink: '/2017/08/24/redux-immutable-js-reselect%e3%81%a7redux-reducer%e3%82%92%e5%ae%89%e5%85%a8%e3%81%8b%e3%81%a4%e7%b0%a1%e6%bd%94%e3%81%ab%e3%81%99%e3%82%8b/'
categories:
  - App
  - IT
  - Programming
  - Web
tags:
  - immutablejs
  - react
  - react-native
  - redux
  - reselect
---
日本語の技術ブログを読んでいると[Immutable.js](https://facebook.github.io/immutable-js/)でmodelを作り、そこにロジックを入れる方法が書かれてるものが多いですが、英語の情報をまでみてみると、それはどちらかというと特殊な方法で多くのところでReduxのReducerに導入しています。

今回は、後者の方法を説明していきます。

## Immutable.js

Reduxのstateはimmutableで操作すべきであり、そのため`Object.assign()`や`slice`等を駆使してReducerを書いていきます。問題はその方法が冗長であり、人によってES6を使わないなど書き方も違うのでコードが見にくくなります。これを解決するのがImuutable.jsになります。ドキュメントを見ると(Typescript前提で書かれてるドキュメントなので、知らないと非常に読み難い...)、便利な関数が沢山生えてます。`setIn`などの関数を使えば、Nestされているオブジェクトに一発でデータを挿入できたりします。そして、そのオブジェクトが常にImmutableであることを保証してくれます。

Immutablejsのreducerへの組み込み方は簡単で、InitialStateをimmutableJSで作れるだけです。

```
import { ADD_TITLE, ADD_ELEMENT } from "../actions/draft";
import { Map, fromJS } from "immutable";

const initialState = fromJS({
  title: null,
  element: null,
  standards: {}, 
  answers: {}
});

export default function draft(state = initialState, action) {
  switch (action.type) {
    case ADD_TITLE:
      return state.set("title", action.title); // 直感的かつ簡潔！
    case ADD_ELEMENT:
      return state.set("element", action.element);
    default:
      return state;
  }
}
```

上記の`state.draft.standards`はImmutable.Mapオブジェクトとなります。問題はこれを`toJS`,`toArray`, `toObject`をコンポーネント内のRedux stateの呼び出しの`mapStateToProps`で使うのがReduxのアンチパターンになっていることです。

>This is a particular issue if you use toJS() in a wrapped component’s mapStateToProps function, as React-Redux shallowly compares each value in the returned props object. For example, the value referenced by the todos prop returned from mapStateToProps below will always be a different object, and so will fail a shallow equality check. 

で、どうするか。ここで登場するのが[Reselect](https://github.com/reactjs/reselect)になります。

## Reselect

[Reselect](https://github.com/reactjs/reselect)は、キャッシュを持ち、変更があった場合だけ新しいオブジェクトを作成するような挙動を記述できます。これを`mapStateToProps`に入れ込むことにより、アンチパターンを避けることができます。　具体的なコードは、


 
<pre class="lang:js decode:true " ># app/selectors/draft.js
import { createSelectorCreator, defaultMemoize } from 'reselect';
import { isEqual } from 'lodash';

const createDeepEqualSelector = createSelectorCreator(
  defaultMemoize,
  isEqual
);

const draftStandardsSelector = (state) =&gt; state.draft.get('standards').toObject();

export const standardsInDraft = createDeepEqualSelector(
  [draftStandardsSelector], // InputSelector(s)
  (standards) =&gt; {
    return standards;
  }
);
</pre> 


詳しくはREADMEや他の技術ブログを参考にしていただければと思いますが、標準の比較関数は単純なオブジェクト比較(`===`)を利用しているため、オブジェクトのkey/valueのペアまで比較できません。ここで`lodash`の`isEqual`を使って、deep比較をすることにしている(今回の場合、shallowな比較(ネストされたものは比較しない)で十分ではあるがREADMEがそうなっているので))。`createDeepEqualSelector`の最初の引数が、Input Selectorと呼ばれるもので、ここが以前のもの(キャッシュ)との比較対象です。`isEqual`コードを読んでいないが直感的にあまりnestしすぎない方が比較自体の計算量が少なくなって良さそうですね。

これをcomponentの`mapStateToProps`で直に呼び出すのは、単純です。

```
import { standardsInDraft } from '../selectors/draft';
...

function mapStateToProps(state, ownProps) {
  const title = state.draft.get("title");
  const element = state.draft.get("element");
  const standards = standardsInDraft(state); // directly calling
  return { title, element, standards };
}
export default connect(mapStateToProps)(Editor);

```

これでreducer内でimmutablejsで快適にstateを操作できるようになりました！


