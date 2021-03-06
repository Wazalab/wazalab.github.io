---
id: 546
title: AWS Ruby SDKでCost Exploreの情報を取得する方法
date: 2018-09-17T06:34:50+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2018/09/17/538-revision-v1/
permalink: /?p=546
---
aws-sdk version3での話です。

### 1. aws-sdkのCost Exploreのモジュールを読み込む

```
gem 'aws-sdk-costexplorer'
```

### 2. IAMユーザに請求情報のアクセス権を与える

上部のナビバーのアカウントをクリックして、請求情報のアクセスのeditをクリックしてIAMアクセスのアクティブ化をチェックして保存。

<img src="https://www.wazalab.com/wp-content/uploads/2018/09/スクリーンショット-2018-09-16-22.29.38.png" alt="" width="819" height="262" class="alignnone size-full wp-image-540" />

### 3. IAMユーザに必要なロールを設定して追加する

下記のロールを作成し(今回はGEtCostAndUsageのみ)、該当ユーザでロールを追加する
 
<pre class="lang:js decode:true " >{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ce:GetCostAndUsage",
      "Resource": "*"
    }
  ]
}</pre> 


### 4. REPLでテスト

Rails cでコンソールを呼び出しテスト。各キーは設定済みとする。

初期化。us-east-1でのみ提供されている

 
<pre class="lang:ruby decode:true " >c = Aws::CostExplorer::Client.new(region: "us-east-1")　</pre> 


BlendedCostとUnblendedCostの差がわかりづらい。Unblendedの方が割引などを除いた実際の数値に近いという理解で良いのか。

[ドキュメント](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/CostExplorer/Client.html)を読み試行錯誤の結果、タグに紐づいたコストを引くことができた。

 
<pre class="" >

c.get_cost_and_usage({time_period: {start: "2018-09-01", end: "2018-09-15"}, granularity: 'DAILY', metrics: ['UnblendedCost'], filter: {tags: {key: 'UserId', values: ["1"]}}, group_by: [{type: 'DIMENSION' ,key: 'SERVICE'}] })
...
#<struct Aws::CostExplorer::Types::ResultByTime
time_period=#<struct Aws::CostExplorer::Types::DateInterval start="2018-09-10", end="2018-09-11">,
total={},
groups=
[#<struct Aws::CostExplorer::Types::Group keys=["EC2 - Other"], metrics={"UnblendedCost"=>#<struct Aws::CostExplorer::Types::MetricValue amount="0.1999999992", unit="USD">}>,
#<struct Aws::CostExplorer::Types::Group keys=["Amazon Elastic Compute Cloud - Compute"], metrics={"UnblendedCost"=>#<struct Aws::CostExplorer::Types::MetricValue amount="0.0192", unit="USD">}>],
estimated=true>,
#<struct Aws::CostExplorer::Types::ResultByTime
time_period=#<struct Aws::CostExplorer::Types::DateInterval start="2018-09-11", end="2018-09-12">,
total={},
groups=
[#<struct Aws::CostExplorer::Types::Group keys=["EC2 - Other"], metrics={"UnblendedCost"=>#<struct Aws::CostExplorer::Types::MetricValue amount="0.1999999992", unit="USD">}>,
#<struct Aws::CostExplorer::Types::Group keys=["Amazon Elastic Compute Cloud - Compute"], metrics={"UnblendedCost"=>#<struct Aws::CostExplorer::Types::MetricValue amount="0.110821", unit="USD">}>],
...
</pre> 

### 注意

Cost ExploreのAPIリクエストにはお金がかかりそう。。

<img src="https://www.wazalab.com/wp-content/uploads/2018/09/スクリーンショット-2018-09-16-22.25.37.png" alt="" width="895" height="174" class="alignnone size-full wp-image-539" />
