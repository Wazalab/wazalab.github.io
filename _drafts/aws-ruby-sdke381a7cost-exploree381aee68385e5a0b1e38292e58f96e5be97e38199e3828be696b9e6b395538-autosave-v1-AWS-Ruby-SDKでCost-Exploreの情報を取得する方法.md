---
id: 544
title: AWS Ruby SDKでCost Exploreの情報を取得する方法
date: 2018-09-16T22:34:08+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2018/09/16/538-autosave-v1/
permalink: /?p=544
---
aws-sdk version3での話です。

<h3>1. aws-sdkのCost Exploreのモジュールを読み込む</h3>

<pre><code>gem 'aws-sdk-costexplorer'
</code></pre>

<h3>2. IAMユーザに請求情報のアクセス権を与える</h3>

上部のナビバーのアカウントをクリックして、請求情報のアクセスのeditをクリックしてIAMアクセスのアクティブ化をチェックして保存。

<img src="https://www.wazalab.com/wp-content/uploads/2018/09/スクリーンショット-2018-09-16-22.29.38.png" alt="" width="819" height="262" class="alignnone size-full wp-image-540" />

<h3>3. IAMユーザに必要なロールを設定して追加する</h3>

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

<h3>4. REPLでテスト</h3>

Rails cでコンソールを呼び出しテスト。各キーは設定済みとする。

初期化。us-east-1でのみ提供されている

<pre class="lang:ruby decode:true " >c = Aws::CostExplorer::Client.new(region: "us-east-1")　</pre>

BlendedCostとUnblendedCostの差がわかりづらい。Unblendedの方が割引などを除いた実際の数値に近いという理解で良いのか。

<a href="https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/CostExplorer/Client.html">ドキュメント</a>を読み試行錯誤の結果、タグに紐づいたコストを引くことができた。

<pre class="" >

c.get_cost_and_usage({time_period: {start: "2018-09-01", end: "2018-09-15"}, granularity: 'DAILY', metrics: ['UnblendedCost'], filter: {tags: {key: 'UserId', values: ["1"]}}, group_by: [{type: 'DIMENSION' ,key: 'SERVICE'}] })
...
#&lt;struct Aws::CostExplorer::Types::ResultByTime
time_period=#&lt;struct Aws::CostExplorer::Types::DateInterval start="2018-09-10", end="2018-09-11"&gt;,
total={},
groups=
[#&lt;struct Aws::CostExplorer::Types::Group keys=["EC2 - Other"], metrics={"UnblendedCost"=&gt;#&lt;struct Aws::CostExplorer::Types::MetricValue amount="0.1999999992", unit="USD"&gt;}&gt;,
#&lt;struct Aws::CostExplorer::Types::Group keys=["Amazon Elastic Compute Cloud - Compute"], metrics={"UnblendedCost"=&gt;#&lt;struct Aws::CostExplorer::Types::MetricValue amount="0.0192", unit="USD"&gt;}&gt;],
estimated=true&gt;,
#&lt;struct Aws::CostExplorer::Types::ResultByTime
time_period=#&lt;struct Aws::CostExplorer::Types::DateInterval start="2018-09-11", end="2018-09-12"&gt;,
total={},
groups=
[#&lt;struct Aws::CostExplorer::Types::Group keys=["EC2 - Other"], metrics={"UnblendedCost"=&gt;#&lt;struct Aws::CostExplorer::Types::MetricValue amount="0.1999999992", unit="USD"&gt;}&gt;,
#&lt;struct Aws::CostExplorer::Types::Group keys=["Amazon Elastic Compute Cloud - Compute"], metrics={"UnblendedCost"=&gt;#&lt;struct Aws::CostExplorer::Types::MetricValue amount="0.110821", unit="USD"&gt;}&gt;],
...
</pre>

<h3>注意</h3>

Cost ExploreのAPIリクエストにはお金がかかりそう。。

<img src="https://www.wazalab.com/wp-content/uploads/2018/09/スクリーンショット-2018-09-16-22.25.37.png" alt="" width="895" height="174" class="alignnone size-full wp-image-539" />