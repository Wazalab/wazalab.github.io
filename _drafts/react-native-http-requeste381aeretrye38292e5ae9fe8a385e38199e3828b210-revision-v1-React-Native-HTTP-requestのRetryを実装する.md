---
id: 213
title: '[React Native] HTTP requestのRetryを実装する'
date: 2017-06-02T08:48:28+09:00
author: Shohei
layout: revision
guid: http://www.wazalab.com/2017/06/02/210-revision-v1/
permalink: /?p=213
---
Mediumに英語のエントリ[Retry POST request when it’s failed on React Native](https://medium.com/@shohey1226/retry-post-request-when-its-failed-on-react-native-63059eae5c0d)を書いた内容と同じですが、地下鉄で地下に潜ると[react-native-oauth](https://github.com/fullstackreact/react-native-oauth)の`makeRequest`が失敗するケースが発生しました。Twitterの2つ目の投稿が投げられていなかったり、FBへの投稿も落ちていました。そこで、リトライするコードを書きました。

[Async Retry](https://github.com/zeit/async-retry)を使います。これは[node-retry](https://github.com/tim-kos/node-retry)をラップしたもので、例がシンプルで良さそうだったので使います。(async/awaitの文法にも慣れているので）


 
<pre class="theme:terminal lang:default decode:true " > $ cd PROJECT_DIR
 $ yarn add async-retry # or npm install async-retry --save</pre> 



問題なくインストールできます。そして、コードは、下記のような感じ。

```
import retry from 'async-retry'

...

  async _tweetWithRetry(text){
    return await retry(async () => {
      let encodedText = encodeURIComponent(text);
      const endpoint = `https://api.twitter.com/1.1/statuses/update.json?status=${encodedText}`;
      const res = await manager.makeRequest('twitter', endpoint, { method: 'post' });
      return res;
    }, {
      retries: 5
    });
  },
```

2つ目の引数に[node-retry](https://github.com/tim-kos/node-retry)のオプションを書いてくイメージです。今回はエラーハンドリングは特にしないで5回リトライする単純なコードです。iphoneのデベロッパーのところにある"Very bad network"で試したところ、コンソールに数回トライしているのを確認しました。(Testをうまく書けるのかな。。）

以上、リトライを実装したい方は参考にしてみてください。

