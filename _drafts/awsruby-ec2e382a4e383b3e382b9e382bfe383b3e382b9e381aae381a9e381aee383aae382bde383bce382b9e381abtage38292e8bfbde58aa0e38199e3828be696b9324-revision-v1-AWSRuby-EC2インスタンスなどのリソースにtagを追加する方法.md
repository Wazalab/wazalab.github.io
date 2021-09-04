---
id: 328
title: '[AWS][Ruby] EC2インスタンスなどのリソースにtagを追加する方法'
date: 2018-08-23T21:58:45+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2018/08/23/324-revision-v1/
permalink: /?p=328
---
AWSのRuby SDK version3での話。

Gemfileに下記を追加して、
 
<pre class="theme:monokai lang:ruby decode:true " >gem 'aws-sdk-ec2'</pre> 

`create_tag` methodを呼べば良い。Resouceで各リソースのID系を入れればうまくやってくれそう。

<pre class="theme:monokai lang:ruby decode:true " >  def add_tag
    begin
      resp = _client.create_tags({
        resources: [
          self.instance_id
        ],
        tags: [
          {
            key: "UserId",
            value: "#{self.user.id}",
          },
        ],
      });
      logger.debug resp
      true
    rescue =&gt; error
      logger.error error.inspect
      false
    end
  end

  private
    def _client
      @client ||= Aws::EC2::Client.new(region: "ap-northeast-1") 
    end</pre> 

