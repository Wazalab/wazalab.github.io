---
id: 735
title: '[Rails] Action Mailerのdelivery_methodを動的に切り替える方法'
date: 2019-05-31T16:13:50+09:00
author: Shohei
layout: post
guid: https://www.wazalab.com/?p=735
permalink: '/2019/05/31/rails-action-mailer%e3%81%aedelivery_method%e3%82%92%e5%8b%95%e7%9a%84%e3%81%ab%e5%88%87%e3%82%8a%e6%9b%bf%e3%81%88%e3%82%8b%e6%96%b9%e6%b3%95/'
siteorigin_page_settings:
  - 'a:6:{s:6:"layout";s:7:"default";s:10:"page_title";b:1;s:15:"masthead_margin";b:1;s:13:"footer_margin";b:1;s:16:"display_masthead";b:1;s:22:"display_footer_widgets";b:1;}'
categories:
  - IT
  - Programming
tags:
  - actionmailer
  - api
  - rails
  - sendgrid
---
SendGridのAPI経由のEメール送信がテンプレートを使えて非常に良いので切り替えたいが、現在smtpで送っているメールをすべて一回で切り替えるのは不可能なので、同時運用したい。
下記のように、`delivery_method`と`delivery_method_options`をmailメソッドに入れるとwrapしてくれるので、smtpを使わなくなる。
 
<pre class="lang:ruby decode:true " >  def send_test_mail
    mail(
      to: 'myemail@change.this.com', 
      subject: 'email subject', 
      body: 'not used',
      template_id: "xxxx",
      delivery_method: :sendgrid_actionmailer, 
      delivery_method_options: {
        api_key: ENV['SENDGRID_API_KEY'] 
      }
    )
  end</pre> 

