---
id: 626
title: AWS Load Balancer(ALB)経由だとコネクションが切れる問題を解決する
date: 2018-10-10T22:05:56+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2018/10/10/624-revision-v1/
permalink: /?p=626
---
ALBにはタイムアウトが設定されており、デフォルトでは60秒でwebsocket経由のアプリなどはがんがん再接続が起きてしまう。単純にこれを変更すれば良い。1-4000秒の値を設定できる。

Web UIからLoad Balancer、該当Load　BalancerのDescriptionの下のeditを編集すればよい。

<img src="https://www.wazalab.com/wp-content/uploads/2018/10/d6ac231e-8a22-4950-9ccc-8fcd9c7553a2.png" alt="" width="570" height="292" class="alignnone size-full wp-image-625" />