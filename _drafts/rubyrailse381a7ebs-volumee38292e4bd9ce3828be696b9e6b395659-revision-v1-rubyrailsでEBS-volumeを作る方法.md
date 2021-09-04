---
id: 662
title: ruby/railsでEBS volumeを作る方法
date: 2019-01-09T09:41:48+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2019/01/09/659-revision-v1/
permalink: /?p=662
---
AWS version3でebs volumeを作るのはドキュメント通り。
 
<pre class="lang:ruby decode:true " >  def create_volume
    begin
      resp = ec2_client.create_volume({
        availability_zone: self.availability_zone,
        size: self.size,
        volume_type: "gp2"
      })
      logger.debug resp
      self.update(volume_id: resp.volume_id)
    rescue =&gt; error
      logger.error error.inspect
      false
    end  end

  def delete_volume
    begin
      resp = ec2_client.delete_volume({
        volume_id: self.volume_id      })
      logger.debug resp
      self.destroy
    rescue =&gt; error      logger.error error.inspect
      false
    end
  end

  def ec2_client
    @client ||= Aws::EC2::Client.new(region: self.region) # "ap-northeast-1"
  end

  def region
    "ap-northeast-1"
  end</pre> 
