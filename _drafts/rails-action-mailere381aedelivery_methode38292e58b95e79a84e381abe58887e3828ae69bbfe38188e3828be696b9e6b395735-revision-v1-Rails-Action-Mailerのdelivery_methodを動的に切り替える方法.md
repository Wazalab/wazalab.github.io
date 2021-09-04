---
id: 736
title: '[Rails] Action Mailerのdelivery_methodを動的に切り替える方法'
date: 2019-05-31T16:13:50+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2019/05/31/735-revision-v1/
permalink: /?p=736
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

