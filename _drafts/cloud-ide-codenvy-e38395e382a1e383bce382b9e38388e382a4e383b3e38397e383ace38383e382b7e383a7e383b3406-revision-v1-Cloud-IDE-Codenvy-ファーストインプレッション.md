---
id: 434
title: '[Cloud IDE] Codenvy ファーストインプレッション'
date: 2018-08-28T07:08:06+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2018/08/28/406-revision-v1/
permalink: /?p=434
---
redhatが買収したCodenvyです。Eclipse Cheを利用したIDEでEcliseユーザに馴染み深いものになっているのではないでしょうか。

一言でまとめるなら、"2018年だったらCodenvy使うよね"という感じです。程よくインフラが抽象化されていて開発環境を提供するという目的に則していると思います。

スクショで見ていきます。

### Socialボタンによるサインアップ・ログインを完備

Email/passだけではなく、githubログインなどがあるのは良いですね。
<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-26-9.01.55-1024x574.png" alt="" width="1024" height="574" class="alignnone size-large wp-image-408" />

今回はgithubログインを選択。ユーザデータ、sshキー、レポジトリへのアクセスが取られます。githubを利用した開発フローならば後で必要になると思うので許可します。
<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-26-9.04.48-1024x535.png" alt="" width="1024" height="535" class="alignnone size-large wp-image-409" />

プロフィールを設定してサインアップ終了
<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-26-9.05.23-1024x649.png" alt="" width="1024" height="649" class="alignnone size-large wp-image-410" />

### 3GのRAMが無料で使える！

太っ腹ですね。
<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-26-9.06.15-1024x583.png" alt="" width="1024" height="583" class="alignnone size-large wp-image-411" />

### Workspace毎にVMが立ち上がる

Cloud9などのサービスだと開発するサーバを意識して作らないといけませんが、workspaceという名前になっていてサーバ側を意識しない環境になっています。

<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-26-9.06.28-1024x210.png" alt="" width="1024" height="210" class="alignnone size-large wp-image-412" />

ワークスペースの設置でどの開発環境で行うか選べる。
<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-26-9.06.40-1024x504.png" alt="" width="1024" height="504" class="alignnone size-large wp-image-413" />

Herokuみたいに環境をぽちぽち選べる。これはすごい。Railsもありました。
<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-26-9.07.23-1024x426.png" alt="" width="1024" height="426" class="alignnone size-large wp-image-414" />

選べるstack。
<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-26-9.38.05-1024x563.png" alt="" width="1024" height="563" class="alignnone size-large wp-image-421" />

### Dockerによる開発環境提供

作成するとdockerのイメージが立ち上がる。30秒ぐらいで立ち上がります。
<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-26-9.08.26-1024x315.png" alt="" width="1024" height="315" class="alignnone size-large wp-image-415" />

<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-26-9.08.29-1024x578.png" alt="" width="1024" height="578" class="alignnone size-large wp-image-416" />

### 開発環境はUbuntu
<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-26-9.11.54-1024x347.png" alt="" width="1024" height="347" class="alignnone size-large wp-image-417" />

4Gストレージ付き
<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-26-9.12.47.png" alt="" width="954" height="188" class="alignnone size-large wp-image-418" />


### 10分使用しないと停止設定

おそらくこれが課金ポイントでしょう。10分なんてロジックを考えてたり、同僚とdiscussionしてたら過ぎてしまいますしね。それ毎に30秒待つの苦しい。
<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-26-9.16.17-1024x373.png" alt="" width="1024" height="373" class="alignnone size-large wp-image-419" />

### Projectを作って開始

残念なのはRailsのプロジェクトがない。。Eclipse開発系多めという印象でしょうか。

<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-26-9.19.16-1024x725.png" alt="" width="1024" height="725" class="alignnone size-large wp-image-420" />

### まとめ

* クレジットカードを入れることなく始められる
* 3GRAMまで無料で利用できる(2018年8月時点)
* 環境設定を一からやる必要のないStackという概念
* インフラ（サーバー）を意識しない設計
* Eclipse好きな人には良さそう