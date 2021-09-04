---
id: 261
date: 2017-08-24T12:32:45+09:00
author: Shohei
layout: revision
guid: http://www.wazalab.com/2017/08/24/260-revision-v1/
permalink: /?p=261
---
# Redux + immutable.js + Reselectで快適にredux stateを操作する

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
      return state.set("title", action.title);
    case ADD_ELEMENT:
      return state.set("element", action.element);
    default:
      return state;
  }
}
```

上記の`state.draft.standards`はImmutable.Mapオブジェクトとなります。問題はこれを`toJS`,`toArray`, `toObject`をコンポーネント内のRedux stateの呼び出しの`mapStateToProps`で使うのがReduxのアンチパターンになっていることです。

>This is a particular issue if you use toJS() in a wrapped component’s mapStateToProps function, as React-Redux shallowly compares each value in the returned props object. For example, the value referenced by the todos prop returned from mapStateToProps below will always be a different object, and so will fail a shallow equality check. 

で、どうするか。ここで登場するのが[reselect](https://github.com/reactjs/reselect)になります。

## reselect

reselectは、キャッシュを持ち、変更があった場合だけ新しいオブジェクトを作成するような挙動を記述できます。これを`mapStateToProps`に入れ込むことにより、アンチパターンを避けることができます。　具体的なコードは、

```
# app/selectors/draft.js
import { createSelectorCreator, defaultMemoize } from 'reselect';
import { isEqual } from 'lodash';

const createDeepEqualSelector = createSelectorCreator(
  defaultMemoize,
  isEqual
);

const draftStandardsSelector = (state) => state.draft.get('standards').toObject();

export const standardsInDraft = createDeepEqualSelector(
  [draftStandardsSelector], // InputSelector(s)
  (standards) => {
    return standards;
  }
);
```

詳しくはREADMEや他の技術ブログを参考にしていただければと思うが、標準の比較関数は単純なオブジェクト比較(`===`)を利用しているため、オブジェクトのkey/valueのペアまで比較できない。ここで`lodash`の`isEqual`を使って、deep比較をすることにしている(今回の場合、shallowな比較(ネストされたものは比較しない)で十分ではあるがREADMEがそうなっているので))。`createDeepEqualSelector`の最初の引数が、Input Selectorと呼ばれるもので、ここが以前のもの(キャッシュ)と比較対象になる。`isEqual`コードを読んでいないが直感的にあまりnestしすぎない方が比較自体の計算量が少なくなって良さそうに思う。

これをcomponentの`mapStateToProps`で直に呼び出す。

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

これでreducer内でimmutablejsで快適にstateを操作できるようになりました。


