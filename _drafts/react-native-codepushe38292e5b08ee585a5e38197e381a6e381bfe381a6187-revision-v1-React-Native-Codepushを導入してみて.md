---
id: 201
title: '[React Native]  Codepushを導入してみて'
date: 2017-05-09T09:24:21+09:00
author: Shohei
layout: revision
guid: http://www.wazalab.com/2017/05/09/187-revision-v1/
permalink: /?p=201
---
# [Codepush](https://microsoft.github.io/code-push/)

Mircosoftが提供するプラットフォームで、CordovaとReactNative用のバイナリを変えずに該当コードだけを変更するものです。React Nativeの場合は、JSバンドルだけをダウンロードして差し替えることができます。何がうれしいのかというとレビューを通さずにコードを変更*1することです。とはいえ、最近は3日以内にAppleのレビューが終わりますし、そんなに必要ではない仕組みかもしれません。


実際導入してみると、運用のオーバーヘッドは大きめです。数人で開発・運用しているアプリなら問題ないと思いますが、個人プロジェクトだとメンテが大変です。そして、気づくことはitunes connectやGoogle playでの変更は必要ということです。イメージ的に大きめの変更はアプリレビューを通して、小さなもの、もしくは致命的なバグはCodepushしてくのが実際の使いどころになりそうだと感じています。


*1 アプリの意図を変えないようにしましょう。

では、さっそくインストールしましょう。

## インストール

README通りです。

CLIインストール

 
<pre class="theme:dark-terminal lang:default decode:true " >
$ npm install -g code-push-cli
</pre> 


<pre class="theme:dark-terminal lang:default decode:true " >
$ code-push register
A browser is being launched to authenticate your account. Follow the instructions it displays to complete your registration.

Enter your access key:
</pre>

<pre class="theme:dark-terminal lang:default decode:true " >
$ code-push app add Kagami
</pre>

Deployment keyが表示される。
READMEとは前後してしまいますが、下記のようなことが後で言われるので, iosとandroid両方作りたい場合は、両方のアプリを作っておきます。

&gt;Note, if you are targeting both platforms it is recommended to create separate CodePush applications for each platform.

<pre class="theme:dark-terminal lang:default decode:true " >
$ code-push app add Kagami_android
Successfully added the "Kagami_android" app, along with the following default deployments:
┌────────────┬───────────────────────────────────────┐
│ Name       │ Deployment Key                        │
├────────────┼───────────────────────────────────────┤
│ Production │  YOURS                                │
├────────────┼───────────────────────────────────────┤
│ Staging    │ 	YOURS                                │
└────────────┴───────────────────────────────────────┘
</pre>

## アプリ側の設定

Githubに飛んでReadme通りに設定を進める。注意点としては、READMEがReact Nativeのバージョンごとに違うこと。 自分が使ってるReactNativeに相当するブランチをTagから選択。（ということは、将来的にRNをアップグレードしたらCodePushも変更しなくてはいけない。。）

![](https://cloud.githubusercontent.com/assets/8598682/17350832/ce0dec40-58de-11e6-9c8c-906bb114c34f.png)

<pre class="theme:dark-terminal lang:default decode:true " >
$ yarn add react-native-code-push@latest
# このままだと最新のものがインストールされる。使用してるReactNative環境によってバージョンを変えるのを忘れないように。
$ vi package.json
// change the line if required
"react-native-code-push": "1.16.1-beta",
$ yarn install
</pre>

このあとios/androidの設定ファイル(xcode, gradle)の変更する。ここは各READMEを参照のこと。

アプリにcodeプッシュを適応するのは簡単で、単純にWrapするだけ。
Reduxを利用してるので少しReadmeとは違うが、単純にRegisterComponentの前にラップするだけ問題ない。

```
# index.ios.js
import React  from 'react';
import { AppRegistry } from 'react-native';
import Route from './app/Route';
import codePush from "react-native-code-push";

let Kagami = React.createClass({
  render: function() {
    return(
      <Route />
    );
  }
});

let codePushOptions = { 
  checkFrequency: codePush.CheckFrequency.ON_APP_RESUME,  
  installMode: codePush.InstallMode.ON_NEXT_RESUME,
};

Kagami = codePush(codePushOptions)(Kagami)

AppRegistry.registerComponent('Kagami', () => Kagami);
```

また`codePushOptions`でオプションを指定できる。上記は、悩んだ末に決めたバックグランドからの復帰時にアプリのバイナリを確認・そして次期バージョンがあったら、次のバックグランドからの復帰時にインストールする。

すぐにインストールする等もできるが、バスンとアプリが落ちて起動する感じになるので、ユーザは気分がよくないでしょう。。

#### アプリのリリース方法

<pre class="theme:dark-terminal lang:default decode:true " >
$ code-push release-react Kagami ios
$ code-push release-react Kagami_android android
</pre>

これでStaging環境にアップロードされる。
ベータ版を端末に入れておいて、テストしてみて、オッケーなら本番環境にあげる。

<pre class="theme:dark-terminal lang:default decode:true " >
$ code-push promote Kagami_android Staging Production
</pre>

## 開発環境

README通りにやると下記のようになる。

### iOS

xcode project schema

* debug: ローカルでシミュレーターを利用して開発する。code pushは利用しない。
* staging: SchemaをStagingに変更し、xcodeからデバイスへ転送。iPhoneにStagingバイナリを転送済の場合、USBなしで実機テストができる。
* production: SchemaをStagingに変更し、xcodeからデバイスへ転送。または、アーカイブしてitunes connectにアップロード。エンドユーザが使う。iTunes storeから配布されるアプリが参照する。

### Android

* debug: `react-native run-android`でemulatorでテストする。
* releaseStaging: `react-native run-android --variant=releaseStaging`
* release: `cd android && ./gradlew assembleRelease` でAPKをGoogle Playにアップロード。

`android/app/build.gradle`は下記のようになるはず。

```
buildTypes {
  debug {
    buildConfigField "String", "CODEPUSH_KEY", '""'
    applicationIdSuffix ".debug"
  }
  releaseStaging {
    signingConfig signingConfigs.release
    buildConfigField "String", "CODEPUSH_KEY", '"XXXXXX"'
    applicationIdSuffix ".staging"
  }
  release {
    minifyEnabled enableProguardInReleaseBuilds
    proguardFiles getDefaultProguardFile("proguard-android.txt"), "proguard-rules.pro"
    buildConfigField "String", "CODEPUSH_KEY", '"YYYYYYYY"'
   signingConfig signingConfigs.release
  }
}

```

## Deployment状況の確認

<pre class="theme:dark-terminal lang:default decode:true " >
$ code-push deployment ls Kagami_android
$ code-push deployment ls Kagami
</pre>

<pre class="theme:dark-terminal lang:default decode:true " >
$ code-push deployment ls Kagami
┌────────────┬───────────────────────────────┬────────────────────────┐
│ Name       │ Update Metadata               │ Install Metrics        │
├────────────┼───────────────────────────────┼────────────────────────┤
│ Production │ Label: v12                    │ Active: 2.6% (1 of 39) │
│            │ App Version: 1.5              │ Total: 1 (1 pending)   │
│            │ Mandatory: No                 │                        │
│            │ Release Time: 3 days ago      │                        │
│            │ Released By: shoheik@cpan.org │                        │
├────────────┼───────────────────────────────┼────────────────────────┤
│ Staging    │ Label: v31                    │ No installs recorded   │
│            │ App Version: 1.5              │                        │
│            │ Mandatory: No                 │                        │
│            │ Release Time: 4 days ago      │                        │
│            │ Released By: shoheik@cpan.org │                        │
└────────────┴───────────────────────────────┴────────────────────────┘
</pre>

### AndroidのProduction環境のバイナリの作り方 

<pre class="theme:dark-terminal lang:default decode:true " >
$ cd android && ./gradlew assembleRelease
</pre>

リリース方法はiosと同じ

<pre class="theme:dark-terminal lang:default decode:true " >
$ code-push release-react Kagami_android android
$ code-push promote Kagami_android Staging Production
</pre>

## Troubleshooting

基本的に難しい。迷った場合は、下記を確認する。

* Versionが正しくない
* オプションのパラメータが正しくない
* 同じバージョンのバイナリだとプッシュされない（？）

デフォルトパラメータで設定する場合、Pushしたときに、そのオプションが次のcodepushの振る舞いになるので。現在codepush側のパラメータが何であるか覚えておかないといけません。


## まとめ

ただでさえ、React Nativeのバージョンに追いつくのに必死なのに、Codepushのバージョンも考えないといけないのはつらい。起動時にcodepush側に最新があるか見にいく仕様なので、app storeで配布しているバイナリに欠陥があってはいけない。例えば、Sign UpのViewにバグをcodepushでは直しにくい。なぜなら、Resumeの場合、バックグランドから戻るタイミングがないし、再起動を強制するのは、（ダウンロードする時間があるので）サインアップの途中で再起動してしまう可能性がある。またApplinkでFacebookログインを外部に行く場合は、そこでResumeが起こってしまうのも注意。つまり、Facebook認証後戻ってくるときにアプリが再起動してしまう。


やはり、使い所はApp storeのレビューが終わるまで直せないような致命的なバグ。赤い画面で落ちてしまうようなバグはリスタートが必要なので、緊急パッチを当てることができる。またStaging環境の実機テストもパッケージをアップロードしなくていいので楽になる。


codepushをセットアップしても必ず使う必要はない。release等がなくても元のバイナリでユーザは使えることができる。なので、余力のあるチームは、導入を検討してみてはどうでしょうか？