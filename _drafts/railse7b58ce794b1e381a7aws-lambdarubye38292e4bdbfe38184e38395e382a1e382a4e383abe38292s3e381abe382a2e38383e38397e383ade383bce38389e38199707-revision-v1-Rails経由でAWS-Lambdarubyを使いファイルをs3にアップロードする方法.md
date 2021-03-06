---
id: 720
title: Rails経由でAWS Lambda(ruby)を使いファイルをs3にアップロードする方法
date: 2019-04-18T13:05:53+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2019/04/18/707-revision-v1/
permalink: /?p=720
---
タイトルが渋滞していますが、、やったことは下記のこと。

1. Railsが非同期ジョブ走らせる
2. 非同期ジョブはlambdaをinvokeする
3. lambdaがcurlでファイルを持ってきて、s3にアップロードする
4. その結果のアップロード先URLをRailsのDBに保存

AWS Lambdaを触ったのは初めてです。nodeではなくrubyで動かします。まずは、AWS上にLambda関数を作ります。今回は、コードは短いだろうと予想し、ローカルにインストールせずにAWSのWEB UIのエディタを使いました。

## Role 追加

登場ロールは２つ、Rails側でlambdaをinvokeするロールとlambda関数を走らせるロール

#### Rails側でlambdaをinvokeするロール
*全部必要ではないかもしれませんが、面倒なので全て権限をあげました。
<img src="https://www.wazalab.com/wp-content/uploads/2019/04/roles-1.png" alt="" width="459" height="189" class="alignnone size-full wp-image-716" />

#### lambda関数を走らせるロール
<img src="https://www.wazalab.com/wp-content/uploads/2019/04/role1.png" alt="" width="381" height="147" class="alignnone size-full wp-image-710" />

## Rails側のコード

AWS Ruby SDK version2 です。RequestResponseだと同期で処理します。返り値を拾うためにもここは同期で処理。invoke_copy自体を非同期で走らせるイメージです。
 
<pre class="lang:ruby decode:true " >  def invoke_copy(recording_id, download_url, object_path)
    req_payload = {
      download_url: download_url, 
      object_path: object_path, 
    }
    payload = JSON.generate(req_payload)
    resp = lambda.invoke({
      function_name: "copyToS3", # required
      invocation_type: "RequestResponse", # accepts Event, RequestResponse, DryRun
      log_type: "Tail", # accepts None, Tail
      payload:  payload,
    })
    logger.debug Base64.decode64(resp.log_result)
    res = JSON.parse(resp["payload"].read());
    logger.debug res
    self.update(url: res["body"])
  end

  def lambda
    @lambda ||= Aws::Lambda::Client.new(
      region: "ap-northeast-1",
      access_key_id: ENV['AWS_ACCESS_KEY_ID'], 
      secret_access_key: ENV['AWS_SECRET_KEY'] 
    )
  end</pre> 

## Lambda関数のコード

AWS Ruby SDK version3です。Lambdaの/tmpのみがwritableな場所で、maxで512MBです。
したがって、これ以上コピーできません。

 
<pre class="lang:ruby decode:true " >require 'json'
require 'aws-sdk-s3'

def lambda_handler(event:, context:)
    puts event
    pp ENV
    puts `curl -L #{event["download_url"]} -o /tmp/a`
    puts `ls -al /tmp/a`
    puts `file /tmp/a`
    s3 = Aws::S3::Resource.new
    obj = s3.bucket('my-backet').object(event["object_path"])
    obj.upload_file('/tmp/a')
    
    { statusCode: 200, body: obj.public_url }
end</pre> 

