---
id: 267
title: immutableJSのMapをredux-persistで保存できないときの対処法
date: 2017-09-12T08:52:13+09:00
author: Shohei
layout: post
guid: http://www.wazalab.com/?p=267
permalink: '/2017/09/12/immutablejs%e3%81%aemap%e3%82%92redux-persist%e3%81%a7%e4%bf%9d%e5%ad%98%e3%81%a7%e3%81%8d%e3%81%aa%e3%81%84%e3%81%a8%e3%81%8d%e3%81%ae%e5%af%be%e5%87%a6%e6%b3%95/'
categories:
  - App
  - IT
  - Programming
tags:
  - immutablejs
  - react-native
  - redux
  - redux-persist
---
[以前説明した](http://www.wazalab.com/2017/08/24/redux-immutable-js-reselect%e3%81%a7redux-reducer%e3%82%92%e5%ae%89%e5%85%a8%e3%81%8b%e3%81%a4%e7%b0%a1%e6%bd%94%e3%81%ab%e3%81%99%e3%82%8b/)immutableJSは簡単にimmutableオブジェクトを扱うことができるのでreduxのreducerで大活躍します。
normalizr風味のreducerを作る場合は、下記のようにimmutableJSのMapとListで簡潔に書く事ができます。

```
import { ADD_THOUGHT } from "../actions/thought";
import { Map, fromJS } from "immutable";

const initialState = fromJS({
  ids: [],
  entities: {}
});

export default function thought(state = initialState, action) {
  switch (action.type) {
    case ADD_THOUGHT:
      return state
        .setIn(["entities", action.draft.tid], action.draft)
        .set("ids", state.get("ids").push(action.draft.tid));
    default:
      return state;
  }
}
```

一つ問題があります。immutableJSは`fromJS`を利用するとMapオブジェクトでstateを保存します。これが、`redux-persist`を使うとMapで保存できないという問題です。`redux-persist`はtransformsという機構でserialize, deserializeを行うことができ、ここにモジュールを適用することで保存できるようになります。redux-persistのauthorであるr2tzz氏が提供する[redux-persist-immutable](https://github.com/rt2zz/redux-persist-immutable)がありますが、READMEの情報が少なくよくわかりません。他のソースを読むと、トップレベルでMapの置換を適用するようです。私の今回のreducerではnavというReact Navigatorのreducerがあるので、ここに適用する必要はないと思っていました。


## Redux Persist Transform Immutable

[Redux Persist Transform Immutable](https://github.com/rt2zz/redux-persist-transform-immutable)というものもr2tzz氏が提供しています。これは、reducerごとにシリアライズ・デシリアライズをできるものです。これはREADMEもしっかり書いてあります:)

では、実際のコードを見てみましょう。


<pre class="lang:js decode:true " >import immutableTransform from "redux-persist-transform-immutable";

...

  componentWillMount() {
    persistStore(
      this.store,
      {
        storage: AsyncStorage,
        transforms: [
          immutableTransform({
            whitelist: ["user", "draft", "thought"],
            blacklist: ["nav"]
          })
        ]
      },
      async () =&gt; {
        await this.store.dispatch(initUser());
        await this._loadAsync();
      }
    ); //.purge();
  }</pre> 


  
  使い方は、transformsのvalueにimmutableTransformを入れます。whitelist/blacklistで追加します。どちらかを指定すれば片方がいらないかもしれませんが、わかりやすくするためにwhitelist/blacklist両方いれています。React Nativeでの利用なので、`storage: AsyncStorage`はそのままです。 ちなみにpersistStore(..).perge()でstateを消去できるので、開発初期にはよく使うことになると思います。
