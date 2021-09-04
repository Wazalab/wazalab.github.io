---
id: 689
title: RailsでGoogle+シャットダウンに伴うGoogle Sign-in対応
date: 2019-01-31T09:02:16+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2019/01/31/684-revision-v1/
permalink: /?p=689
---
*[Action Required] Google+ APIs and OAuth requests are being shutdown on March 7, 2019*というメールが来ていた。会社でGoogle Sign-Inを使っているので、影響があるかどうか調べて、変更を行った。

まずは、APIコンソールで確認。


<img src="https://www.wazalab.com/wp-content/uploads/2019/01/79feaf71-a6d9-436b-a8eb-e9a4f69a85e1.png" alt="" width="614" height="407" class="alignnone size-full wp-image-685" />

Google+ APIをある程度の頻度で叩いてるので、変更が必要と認識。

Sign-Inに使用しているのGemは、`omniauth-google-oauth2`でgithub issueを探ると最新バージョンではGoogle+ dependencyはないとのこと。したがって、さっそくバージョンアップグレード。


 
<pre class="theme:dark-terminal lang:default decode:true " >bundle update omniauth-google-oauth2
</pre> 


Gemfile.lockを確認すると最新バージョンが入っていない。jwtのバージョンが古いことに起因しているようだ。


 
<pre class="theme:dark-terminal lang:default decode:true " > bundle update google-api-client</pre> 



もうひとつサインインではないが、関係のありそうなgoogle-api-client(主にCalendar APIに利用)のバージョンを上げて、jwtが2.0以上になったことを確認。

もう一度、`bundle update omniauth-google-oauth2`で最新にするもうまくいってなさそうなので、Gemfileにバージョン指定。


 
<pre class="lang:ruby decode:true " >gem "omniauth-google-oauth2", '~&gt;0.6'</pre> 



bundle installして最新のもになってのを確認。

問題の切り分けに新しいプロジェクトをconsole APIに作り、tokenなどを作成し、テスト後、問題がなかったので本番環境にデプロイした。

数時間後、APIコンソールでgoogle+を叩いていないことを確認し、Google+シャットダウンに伴うGoogle Sign-inの対応完了。


所感：Googleに振り回れるがつらい。Hangoutも消えそうな気がするし。。
