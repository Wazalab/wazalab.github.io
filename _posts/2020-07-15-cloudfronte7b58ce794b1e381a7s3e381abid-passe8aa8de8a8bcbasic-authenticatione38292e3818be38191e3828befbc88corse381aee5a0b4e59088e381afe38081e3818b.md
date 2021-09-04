---
id: 875
title: Cloudfront経由でS3にID/Pass認証(Basic Authentication)をかける（CORSの場合は、かけない）方法
date: 2020-07-15T16:37:56+09:00
author: Shohei
layout: post
guid: https://www.wazalab.com/?p=875
permalink: '/2020/07/15/cloudfront%e7%b5%8c%e7%94%b1%e3%81%a7s3%e3%81%abid-pass%e8%aa%8d%e8%a8%bcbasic-authentication%e3%82%92%e3%81%8b%e3%81%91%e3%82%8b%ef%bc%88cors%e3%81%ae%e5%a0%b4%e5%90%88%e3%81%af%e3%80%81%e3%81%8b/'
siteorigin_page_settings:
  - 'a:6:{s:6:"layout";s:7:"default";s:10:"page_title";b:1;s:15:"masthead_margin";b:1;s:13:"footer_margin";b:1;s:16:"display_masthead";b:1;s:22:"display_footer_widgets";b:1;}'
categories:
  - Infrastructure
  - IT
tags:
  - aws
  - cloudfront
  - CORS
  - lambda
  - s3
---
まる二日間格闘して設定が完了した。かなりの試行錯誤したので備忘録としてまとめてみる。
<h2>Goal</h2>
<ul>
 	<li>s3上のファイルにパスワードをかけて直接ファイルをダウンロードできなくする。</li>
 	<li>Webサービスにログインしているユーザは、ファイルをダウンロードできる。</li>
</ul>
<h2>結論</h2>
結論から先に書くと、下記のような感じ。前提として、https + Basic authenticationを信頼する(http + basic authはダメ）。
<ul>
 	<li>Clientに<code>crossorigin="anonymous"</code>をセットし、http requestにorignを含ませる。</li>
 	<li>Cloudfront+LambdaでBasic Authでフィルタリングする。Originを含む場合はパスワード必要としない。</li>
 	<li>S3とCloudfrontではCORS関係のヘッダをパススルーする。</li>
</ul>
<h2>クライアント(Web browser/HTML)の設定</h2>
今回は動画ファイルのダウンロード・再生のUIだったので、下記のようにcrossoriginを設定する。
<pre class="lang:xhtml decode:true ">&lt;video width="100%" height="75%" controls="" crossorigin="anonymous" src="https://my.example.com/xxxxxxx/yy.mp4"&gt;
&lt;/video&gt;</pre>
ちなみに&lt;source src=""&gt;だとうまくoriginが送信されなかった。
<h2>S3のCORSの設定</h2>
いつも悩むCORSの設定。AllowedOriginでソースを限定しておくと安心。
<pre class="lang:xhtml decode:true">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/"&gt;
&lt;CORSRule&gt;
    &lt;AllowedOrigin&gt;https://our.example.com&lt;/AllowedOrigin&gt;
    &lt;AllowedOrigin&gt;https://our.staging.example.com&lt;/AllowedOrigin&gt;
    &lt;AllowedMethod&gt;GET&lt;/AllowedMethod&gt;
    &lt;AllowedMethod&gt;HEAD&lt;/AllowedMethod&gt;
    &lt;MaxAgeSeconds&gt;3000&lt;/MaxAgeSeconds&gt;
    &lt;AllowedHeader&gt;*&lt;/AllowedHeader&gt;
&lt;/CORSRule&gt;
&lt;/CORSConfiguration&gt;
</pre>
<h2>AWS Lambdaの設定</h2>
Requestヘッダにoriginがある場合は、Basic Authは適用せずそのままレスポンスを返す行を追加した。これによし、サーバからのアクセスは許可させる。

<strong>ハマりポイントは、リージョン。S3が東京にあってもバージニアにしないといけない。しかし、ログのCloudwatchは東京リージョンにある。</strong>
<pre class="theme:monokai lang:js decode:true">exports.handler = (event, context, callback) =&gt; {

  // Get the request and its headers
  const request = event.Records[0].cf.request;
  const headers = request.headers;
  
  console.log(headers)
  // if it's from our produciton and stating, no need to do authentication
  const originHost = typeof headers.origin == 'undefined' ? null :  headers.origin[0].value;
  
  if(originHost){
    if(/my\.example\.com/.test(originHost) || /my\.staging\.example\.com/.test(originHost)) {
      callback(null, request);
    }
  }
    

  // Specify the username and password to be used
  const user = 'xxxxxxxxxxxxxxxxxxxxx';
  const pw = 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx';

  // Build a Basic Authentication string
  const authString = 'Basic ' + new Buffer(user + ':' + pw).toString('base64');

  // Challenge for auth if auth credentials are absent or incorrect
  if (typeof headers.authorization == 'undefined' || headers.authorization[0].value != authString) {
    const response = {
      status: '401',
      statusDescription: 'Unauthorized',
      body: 'Unauthorized',
      headers: {
        'www-authenticate': [{key: 'WWW-Authenticate', value:'Basic'}]
      },
    };
    callback(null, response);
  }

  // User has authenticated
  callback(null, request);
};</pre>
<h2>Cloudfrontの設定</h2>
<ul>
 	<li>Basic Authなのでhttpsオンリー</li>
 	<li>CORSにOPTIONSが必要なときがあるらしいので追加</li>
 	<li>whitelistでCORS関連を配信</li>
 	<li>TTLはゼロでテスト</li>
 	<li>下記では省いたが、DNSなどを設定した。デフォルトのCloudfrontのホスト名ではなく自ドメインでアクセスできるように。</li>
 	<li>ViewリクエストにLambdaのARNを追加。<strong>ハマりポイントは、Lambdaのバージョンによって、最後の数字が変わること。Lamdaをpublishしたあとは変更の必要あり。</strong></li>
</ul>
<img class="alignnone size-large wp-image-882" src="https://www.wazalab.com/wp-content/uploads/2020/07/2020-07-15-16-05-console.aws_.amazon.com_-791x1024.jpg" alt="" width="791" height="1024" />
<h2>S3のパーミッション変更</h2>
ここまででCloudfront経由でのアクセスはできるはず。S3に外部からアクセスさせなくするために下記を確認。
<ul>
 	<li>Block <i>all</i> public accessのチェックを外す</li>
 	<li class="ng-scope">
<div class="ng-scope">
<div class="checkbox-tree__header"><span class="ng-scope" translate="lockdown.policies.blockpublicpolicy.title_westeros">Block public access to buckets and objects granted through <i>new</i> public bucket or access point policies -&gt; </span>On</div>
</div></li>
 	<li class="ng-scope">
<div class="ng-scope">
<div class="checkbox-tree__header"><span class="ng-scope" translate="lockdown.policies.lockdownpublicbucket.title_westeros">Block public and cross-account access to buckets and objects through <i>any</i> public bucket or access point policies  -&gt; </span>On</div>
</div></li>
 	<li>Access Control Listにeveryone等権限がないか確認</li>
</ul>
<h2>S3-Cloudfront間の設定</h2>
CloudfrontのOriginのタブからOriginを編集する。
<ul>
 	<li>Restrict Bucket Accessにチェック</li>
 	<li>Original Access Identityで必要ならば新しいものを作成</li>
 	<li>Grand read permissions on Bucketで"Yes, Update Bucket Polity"をチェック</li>
</ul>
<h2>まとめ</h2>
上記でうまくいくはず。デバッグはChromeのdevelopper toolのNetwork tabでHTTP request を確認、consoleエラーがないか確認、lambdaのconsole.logで確認とデータの中身を追っていく必要がある。

&nbsp;