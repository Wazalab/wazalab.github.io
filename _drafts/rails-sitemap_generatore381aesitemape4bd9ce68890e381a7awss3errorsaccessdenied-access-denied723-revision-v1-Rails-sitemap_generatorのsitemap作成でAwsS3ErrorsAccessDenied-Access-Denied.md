---
id: 727
title: '[Rails] sitemap_generatorのsitemap作成でAws::S3::Errors::AccessDenied: Access Denied'
date: 2019-04-23T13:41:16+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2019/04/23/723-revision-v1/
permalink: /?p=727
---
[sitemap_generator gem](https://github.com/kjvarga/sitemap_generator)

sitemap作成でハマったのでメモ。Herokuで動的なsitemapを作る一つの方法として、s3を使ったsitemap配信があります。ググれば、たくさんブログなどあるのですが、aws-sdkのバージョン２を使ったものがメイン。今回はaws sdk version3で構築してたので`Aws::S3::Errors::AccessDenied: Access Denied`が出て時間を無駄に消費しました。

`require 'aws-sdk-s3'`が必要なのはissueを漁ればわかりました。
 
<pre class="lang:ruby decode:true " >#config/sitemap.rb
require 'aws-sdk-s3'
SitemapGenerator::Sitemap.sitemaps_host = "https://s3-ap-northeast-1.amazonaws.com/YOUR_BACKET/"
SitemapGenerator::Sitemap.adapter = SitemapGenerator::AwsSdkAdapter.new(
  "YOUR_BACKET",
  aws_access_key_id: ENV['AWS_ACCESS_KEY_ID'],
  aws_secret_access_key: ENV['AWS_SECRET_ACCESS_KEY'],
  aws_region: 'ap-northeast-1' 
)</pre> 


`Aws::S3::Errors::AccessDenied: Access Denied`ですが、s3のバケットをpublicにしていないと出ていました。なので、コンソールから下記を追加すればOK。

<img src="https://www.wazalab.com/wp-content/uploads/2019/04/1d4b8762-f315-4823-bade-43cd69412b2a-1024x103.png" alt="" width="1024" height="103" class="alignnone size-large wp-image-725" />

