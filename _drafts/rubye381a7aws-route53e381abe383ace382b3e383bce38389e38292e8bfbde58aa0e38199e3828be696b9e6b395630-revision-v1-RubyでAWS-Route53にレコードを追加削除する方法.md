---
id: 678
title: RubyでAWS Route53にレコードを追加/削除する方法
date: 2019-01-23T07:12:48+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2019/01/23/630-revision-v1/
permalink: /?p=678
---
Aws ruby version3 での話です。CLIはjsonを作らないといけなそうですが、rubyだとそこが必要ないので楽。UPSERTはドキュメントによると無ければCREATE、あればUPDATEとのこと。
またactionに`DELETE`を指定することでレコードを削除できる。



 
<pre class="lang:ruby decode:true " >client = Aws::Route53::Client.new(region: "ap-northeast-1")
resp = client.change_resource_record_sets({
  change_batch: {
    changes: [
      {
        action: "UPSERT", 
        resource_record_set: {
          name: "xxx.example.com", 
          resource_records: [
            {
              value: "192.0.2.44", 
            }, 
          ], 
          ttl: 60, 
          type: "A", 
        }, 
      }, 
    ], 
    comment: "Web server for example.com", 
  }, 
  hosted_zone_id: "YOUR_HOST_ZONE", 
})</pre> 

