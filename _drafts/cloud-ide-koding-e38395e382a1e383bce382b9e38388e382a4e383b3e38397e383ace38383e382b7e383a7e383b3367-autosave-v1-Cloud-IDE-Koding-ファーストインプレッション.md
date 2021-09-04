---
id: 405
title: '[Cloud IDE] Koding ファーストインプレッション'
date: 2018-08-26T17:05:06+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2018/08/26/367-autosave-v1/
permalink: /?p=405
---
<a href="https://www.koding.com/">Koding - Modern Dev Environment Delivered</a>

Cloud9との比較で引用されるKodingです。彼らのポジションとしてはCloud　IDEでないと言っていますが、ユーザの主はそれで流入しているのではないでしょうか。

<blockquote>
  Koding is not an Online IDE. ... Koding provisions development environments, makes them accessible locally.
</blockquote>

from <a href="https://blog.koding.com/koding-is-not-an-online-ide-e2693f740ce8">koding blog</a>

"チーム開発を主体とした開発環境を提供する"ことを目標にしている。スタックを自分で作るなど、面倒な部分があります。

スクショで見ていきましょう。

<h3>ユーザ登録にクレジットカードが必要</h3>

このステップはAWSを使うときにも必要ですが、Amazonほど大きくない会社にいきなりクレジットカードを預けるのは少し心配な気がします。あまり、中身を見ないでdeveloper soloを選択しました。（これが原因かあとでチームで新しいアカウントを作成してもスタックを選ばないと進めない状況になりました。。)

<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-25-22.17.57-1024x592.png" alt="" width="1024" height="592" class="alignnone size-large wp-image-373" />

emailとチーム名を入力
<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-25-22.18.31-1024x588.png" alt="" width="1024" height="588" class="alignnone size-large wp-image-374" />

ユーザ・パスワードを入力
<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-25-22.21.59-1024x584.png" alt="" width="1024" height="584" class="alignnone size-large wp-image-377" />

クレジットカード情報を入力
<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-25-22.19.52-1024x545.png" alt="" width="1024" height="545" class="alignnone size-large wp-image-376" />

チームのサブドメインを作成
<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-25-22.18.56-1024x580.png" alt="" width="1024" height="580" class="alignnone size-large wp-image-375" />

<h3>Stack(接続するサーバー）を作る必要がある</h3>

Dveloper soloを選択したのが理由かわかりませんが（あとでチームも作成しましたが同じでした),スタックを選ぶところから始まります。 AWS, Vagrant, Google Cloud Platform, Digital Ocean, Azure, Marathon, softlayerが2018年8月時点で選べるようです。つまり、開発者が開発するサーバを決めなければなりません。

AWSのAPIキーを入力し、Credentialを作成 
<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-25-22.22.38-1024x592.png" alt="" width="1024" height="592" class="alignnone size-large wp-image-378" />

<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-25-22.28.09-1024x747.png" alt="" width="1024" height="747" class="alignnone size-large wp-image-379" />

<h3>AWSのstackを利用するにはAPIキーが必要</h3>

AWSを触ったかとがある人ならIAMでユーザを作って始めればいいとわかりますが、初心者には大変かもしれません。

<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-25-22.31.37-447x1024.png" alt="" width="447" height="1024" class="alignnone size-large wp-image-380" />

stackを作成
<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-25-22.35.53-1024x836.png" alt="" width="1024" height="836" class="alignnone size-large wp-image-381" />

<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-25-22.35.59-1024x822.png" alt="" width="1024" height="822" class="alignnone size-large wp-image-382" />

<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-25-22.36.53-1024x856.png" alt="" width="1024" height="856" class="alignnone size-large wp-image-383" />

<h3>Ace　Editorを利用したコーディング</h3>

Cloud9と同じAceEditorのエディタを利用できます。　設定はCloud9の方が細かく設定できそうです。

<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-25-22.42.58-1024x570.png" alt="" width="1024" height="570" class="alignnone size-large wp-image-385" />

<img src="https://www.wazalab.com/wp-content/uploads/2018/08/スクリーンショット-2018-08-25-22.43.10-1024x570.png" alt="" width="1024" height="570" class="alignnone size-large wp-image-386" />

<h3>まとめ</h3>

<ul>
<li>UIがCool&amp;可愛いので好感がもてる</li>
<li>クレジットカードを最初に入れるのはハードルが高い</li>
<li>一人で開発するにはあまり利点がなさそう。逆にチームでの機能が充実している？（試してません)</li>
<li>Stack(Server)は自分で設定するのである程度インフラの知識が必要</li>
<li>一人でAWSをStackに使うならCloud9の方が良さそう</li>
</ul>