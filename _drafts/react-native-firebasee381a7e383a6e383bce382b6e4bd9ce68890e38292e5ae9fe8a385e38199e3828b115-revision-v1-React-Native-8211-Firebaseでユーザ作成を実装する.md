---
id: 122
title: 'React Native &#8211; Firebaseでユーザ作成を実装する'
date: 2016-11-27T15:49:32+09:00
author: Shohei
layout: revision
guid: http://www.wazalab.com/2016/11/27/115-revision-v1/
permalink: /?p=122
---
Google買収後、Firebaseのドキュメントもマテリアルデザインになりいい感じです。
Firebaseは、RNがでてきてすぐに対応したBaasの一つです。途中で、SDKが使えなくなり、古いバージョンで使うみたいなハックがあったように記憶しています。今は、問題なく動く（はず）です。

今回は、ちゃんと動くどうかを確かめる上でもユーザ作成あたりをさくっと実装してみます。

Firebaseのサイトに行き、アカウントを作成し、プロジェクトを作成して、"ウェブアプリにFirebaseを追加”をクリックしましょう。

<img class="alignnone size-medium wp-image-116" src="http://www.wazalab.com/wp-content/uploads/2016/11/スクリーンショット-2016-11-27-13.31.30-300x115.png" alt="%e3%82%b9%e3%82%af%e3%83%aa%e3%83%bc%e3%83%b3%e3%82%b7%e3%83%a7%e3%83%83%e3%83%88-2016-11-27-13-31-30" width="600" />

すると、下記のようなAPIキー情報もろもろが出てくるのでメモっておきます。
<pre class="lang:default decode:true ">&lt;script src="https://www.gstatic.com/firebasejs/3.6.1/firebase.js"&gt;&lt;/script&gt;
&lt;script&gt;
  // Initialize Firebase
  var config = {
    apiKey: "YOURS",
    authDomain: "YOURS",
    databaseURL: "YOURS",
    storageBucket: "YOURS",
    messagingSenderId: "YOURS"
  };
  firebase.initializeApp(config);
&lt;/script&gt;</pre>
忘れずにEmail認証をenableしておきましょう。

<img class="alignnone size-medium wp-image-117" src="http://www.wazalab.com/wp-content/uploads/2016/11/スクリーンショット-2016-11-27-13.46.03-300x114.png" alt="%e3%82%b9%e3%82%af%e3%83%aa%e3%83%bc%e3%83%b3%e3%82%b7%e3%83%a7%e3%83%83%e3%83%88-2016-11-27-13-46-03" width="600" />

React Nativeプロジェクトに追加するには、

#### 1. Firebaseモジュールのインストール
<pre class="lang:zsh decode:true ">$ yarn add firebase</pre>
早い噂のyarnを使ってますが、`npm install firebase --save`と同義。

### 2. 初期化

この手の外部サービスは、`app/utils/Firebase.js`のようにutils以下に入れるのが私の流儀です。
<pre class="lang:js decode:true ">import * as firebase from 'firebase';

const config = {
  apiKey: "YOURS",
  authDomain: "YOURS",
  databaseURL: "YOURS",
  storageBucket: "YOURS",
  messagingSenderId: "YOURS"
};

module.exports = firebase.initializeApp(config);</pre>
### 3. 読み込んで使用する

今回は利用できるかのテストなので、componentDidMountにハードコードしてますが、ちゃんと使うにはテキストフィードを用意して、emailとパスワードを取得すれば良いと思います。
<pre class="lang:js decode:true ">import firebase from './app/utils/Firebase';

export default class Kagami extends Component {

componentDidMount(){
let email = `test${new Date().getTime()}@test.com`;
let password = "hogehoge";

firebase.auth().createUserWithEmailAndPassword(email, password)
.catch(function(error) {
  // Handle Errors here.
  var errorCode = error.code;
  var errorMessage = error.message;
  if (errorCode == 'auth/weak-password') {
    alert('The password is too weak.');
  } else {
    alert(errorMessage);
  }
  console.log(error);
});
}</pre>
これをreloadすると、下記のようにユーザが作成されたのがわかると思います。

<img class="alignnone size-medium wp-image-118" src="http://www.wazalab.com/wp-content/uploads/2016/11/スクリーンショット-2016-11-27-13.44.23-300x57.png" alt="%e3%82%b9%e3%82%af%e3%83%aa%e3%83%bc%e3%83%b3%e3%82%b7%e3%83%a7%e3%83%83%e3%83%88-2016-11-27-13-44-23" width="600"  />

Firebaseあっさり使えましたね。APIもParseと似ているので楽に使えそうです。