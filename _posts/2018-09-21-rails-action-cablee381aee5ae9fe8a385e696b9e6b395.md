---
id: 580
title: '[Rails] Action Cableの実装方法'
date: 2018-09-21T22:00:06+09:00
author: Shohei
layout: post
guid: https://www.wazalab.com/?p=580
permalink: '/2018/09/21/rails-action-cable%e3%81%ae%e5%ae%9f%e8%a3%85%e6%96%b9%e6%b3%95/'
siteorigin_page_settings:
  - 'a:6:{s:6:"layout";s:7:"default";s:10:"page_title";b:1;s:15:"masthead_margin";b:1;s:13:"footer_margin";b:1;s:16:"display_masthead";b:1;s:22:"display_footer_widgets";b:1;}'
categories:
  - IT
  - Programming
tags:
  - Action Cable
  - javascript
  - JQuery
  - rails
---
JQueryは古いテクノロジーとはいえ、さくっと動くものを作るときは異常に簡単です。
古いテクノロジーは悪ではなく適材適所で使っていくのが大事だと思っています。

まずは、最初からある`app/assets/javascripts/cable.js`を該当layoutで読み込みます。
 
<pre class="lang:js decode:true " ># app/assets/javascripts/application.js
...
//= require cable.js</pre> 

全部のViewでApp.cableを呼ぶ必要はないので`content_for`でActionCableとの通信をとるViewだけJSを走らせるようにします。

<pre class="lang:default decode:true " >
#app/views/layouts/application.html.erb
&lt;head&gt;
 ...
 &lt;%= yield :page_scripts %&gt;
&lt;/head&gt;</pre> 

JSでは、App.cableにイベントを登録していきます。　引数のchannelにはクラス名、引数を渡すとparamsに入ってサーバ側で処理できます。`this.perform`を使ってクラス内のメソッドを呼ぶことができます。development環境では別プロセスで動作確認ができないのでsetTimeoutを用いてテストしています。

<pre class="lang:default decode:true " >&lt;% content_for :page_scripts do %&gt;
&lt;script&gt;
  $(function(){    
    // Subscrubung host status 
    $('span.host-status')
    .filter(function(i){
      return $(this).text() !== "terminated";
    })
    .each(function(i){
      var hostId = $(this).data('hostId');
      var self = this;
      App.cable.subscriptions.create({channel: "HostStatusChannel", host_id: hostId}, {
        connected: function(){                                                          
          console.log('connected');
          //var self = this;                                                            
          //setTimeout(function(){                                                      
          //  console.log('say now');                                                   
          //  self.perform("say");                                                      
          //}, 1000);                                                                   
        },                                                                            
        received: function (data) {                                                   
          var currentStatus = $(self).text();
          if(data.status !== currentStatus){
            $(self).fadeOut(500, function(){
              $(self).text(data.status);
              $(self).fadeIn();
            });
          }
        },                                                                            
      });                                                                             
    });
  }); 
&lt;/script&gt;
&lt;% end %&gt;</pre> 

HTML部分は下記のような感じ。

<pre class="lang:default decode:true " >&lt;span class="host-status" data-host-id="&lt;%= host.id %&gt;"&gt;&lt;%= host.status %&gt;&lt;/span&gt;</pre> 

で最後になりますが、ActionCableを継承したクラスです。subscribedがconnectionとともに走ります。 `stream_for　MODEL`でモデルに関係したチャネルが接続されます。 そして、broadcast_to(MODEL, object)でそのモデルへの接続を確率したチャネルにプッシュすることができます。

 
<pre class="lang:ruby decode:true " >
# app/channels/host_status_channel.rb
class HostStatusChannel &lt; ApplicationCable::Channel
  def subscribed
    host = Host.find(params[:host_id])
    stream_for host 
  end
   def say　# for testing
    HostStatusChannel.broadcast_to(Host.find(1), "hi")
  end
 end
</pre> 

これでダイナミックにDOMを操作できるようになりました。
