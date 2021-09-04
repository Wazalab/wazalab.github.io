---
id: 81
title: React Nativeにおけるローカルデータベースの考察
date: 2016-08-15T08:20:31+09:00
author: Shohei
layout: revision
guid: http://www.wazalab.com/2016/08/15/74-revision-v1/
permalink: /?p=81
---
[前回の同期の話](http://www.wazalab.com/2016/08/13/%E3%83%A2%E3%83%90%E3%82%A4%E3%83%AB%E3%82%A2%E3%83%97%E3%83%AA%E3%81%A8%E3%82%B5%E3%83%BC%E3%83%90%E9%96%93%E3%81%AE%E5%90%8C%E6%9C%9F%E6%96%B9%E6%B3%95-evernote-way/)を考えたときに、どうのようにクライアントのローカルにデータを保存するのかという問題が出てきます。

私が調べた限り、その方法は下記のようになります。

1. ReduxのStoreで管理 -- on Memory
2. AsyncStorageを直接扱う -- on Storage
3. AsyncStorageのラッパー(react-native-store)を使う -- on Storage
4. Native(sqlite)のデータベース(react-native-sqlite-storage)を使う -- on Memory
5. Javascript database(NeDB - react-native-local-mongodb)を使う -- on Memory

当たり前ですが、on Memroyは超高速ですが、容量を気にする必要があります。

### 1. ReduxのStoreで管理 

ReduxのStoreをredux-persistなどを使いAsyncStorageにダンプすること前提です。
いい意味でも悪い意味でも自分で管理する必要が出てきます。具体的には、

* Storeに保存するデータの構造を自前で考える必要がある
* データを取り出すFind、整列のsortのようなメソッドを自前で作る必要がある。
* つまり、自分でデータ構造を変えたときに、すべて書きなおすことになり修羅の道。

データ量が少なく単純な構造のときはReduxのStoreで管理しても問題ないでしょう。

### 2. AsyncStorageを直接扱う 

オフィシャルのDocumentでも直に使うのは推奨されてません。

>It is recommended that you use an abstraction on top of AsyncStorage instead of AsyncStorage directly for anything more than light usage since it operates globally.

問題点は "ReduxのStoreで管理"と同様に自前で作ることですが、それに増してStorageを扱うので処理が遅いことです。この方法にすることは基本的にないでしょう。

### 3. AsyncStorageのラッパーを使う

[react-native-store](https://github.com/thewei/react-native-store)以外にもいくつか同じようなモジュールを見かけました。Findのような関数が付属するので自前でメソッドを書く必要がありません。唯一の欠点は、速度だと思います。react-native-storeのgithub issueでもperformanceが問題になっていました。データ量が増えるとパフォーマンス的に結構厳しそうです。ただAsyncStorageのAPIをプラットフォームが提供すれば、どこでも使えることは選択する上で大事なポイントです。

### 4. Native(sqlite)のデータベースを使う 

iOSやAndroidはsqliteのデータベースが付属しています。このsqliteをブリッジするモジュールを使います。sqliteはメモリ上で動くので高速ですし、メソッド等すべてネイティブですし、最速だと思います。
調べてみると、[react-native-sqlite-storage](https://github.com/andpor/react-native-sqlite-storage)というものがあり、Cordovaの[Cordova-sqlite-storage](https://github.com/litehelpers/Cordova-sqlite-storage)のReact Nativeポートです。これは、iOSとAndroid両方同じAPIでSQLiteを叩けるものです。

Nativeコードのラッパーなので、将来、react-native-macosやreact-native-web、もしくは他プラットフォームが出てきたときに、コードの再利用が他のプラットフォームでできなくなってしまい、その場でreact-native-sqlite-storageのプラットフォームサポートを待つか自分で抽象クラスみたいなものを実装しなければいけないかもしれません。

プラットフォームのターゲットが決まっている場合は、これを選択すべきでしょう。Nativeの利点（メモリ管理など）を思う存分享受できるでしょう。

### 5. Javascript databaseを使う

nodeの世界には、javascriptで実装されたdatabaseというものがあります。例えば、MongoDBの簡略版のNeDBです。React Nativeには[react-native-local-mongodb](https://github.com/smartdemocracy/react-native-local-mongodb)というNodeモジュールからポートされたモジュールがあります。On Memoryで動き、AsyncStorageにPersistすることもできます。Redisみたなものと想像すれば良いでしょうか。逐次AsyncStorageに書き出すのではなく、あるタイミングで書き出します。したがって、データを失う可能性がないとは言えません。

流用性と速度を気にしている場合は選択肢に入ると思いますが、データロスを気にするアプリやメモリの量を気にする場合は避けた方がいいかもしれせん。

## 最後に

用途によって使い分けましょう:)