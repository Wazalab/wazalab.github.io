---
title: AWS ELBのSecurity Groupの設定方法
date: 2021-10-27
author: Shohei
layout: post
parmalink: /2021/10/27/security-group-under-elb/
siteorigin_page_settings:
  - 'a:6:{s:6:"layout";s:7:"default";s:10:"page_title";b:1;s:15:"masthead_margin";b:1;s:13:"footer_margin";b:1;s:16:"display_masthead";b:1;s:22:"display_footer_widgets";b:1;}'
categories:
  - IT
tags:
  - aws
  - security-group 
--- 

ELB/ALBの下にサーバを配置するときにどのセキュリティグループにするかいつもわからなくなるのでメモ。
構成はシンプルで`ELB(https) -> Web(80)`。

### ELBのSecurity Group

* Inbound: HTTPS Custom 0.0.0.0/0  
* Outbound: `WebのSecurity Group` 

### WebのSecurity Group

* Inbound: HTTP Custom 172.31.0.0/16 (VPCのサブネット)
* Outbound: all, custom 0.0.0.0/0 



