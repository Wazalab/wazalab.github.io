---
id: 577
title: '[Rails] Device利用時のconnection.rb(Action Cable)'
date: 2018-09-20T20:54:31+09:00
author: Shohei
layout: post
guid: https://www.wazalab.com/?p=577
permalink: '/2018/09/20/rails-device%e5%88%a9%e7%94%a8%e6%99%82%e3%81%aeconnection-rbaction-cable/'
siteorigin_page_settings:
  - 'a:6:{s:6:"layout";s:7:"default";s:10:"page_title";b:1;s:15:"masthead_margin";b:1;s:13:"footer_margin";b:1;s:16:"display_masthead";b:1;s:22:"display_footer_widgets";b:1;}'
categories:
  - IT
  - Programming
tags:
  - Action Cable
  - devise
  - rails
---
Railsガイドで説明されているconnection.rbはcookieを利用しているもので、deviseを利用する場合は、request.envに設定されているwardenのものを取ってくる必要がある。　もしくはsessionの中を漁る感じ、request.envの方が綺麗に書けるのでこちらを採用した。 (Essentially, the both are cookie tho..)
 
<pre class="lang:ruby decode:true " >module ApplicationCable
  class Connection &lt; ActionCable::Connection::Base
    identified_by :current_user
 
    def connect
      self.current_user = find_verified_user
    end
 
    private
      def find_verified_user
        if verified_user = request.env['warden'].user 
          verified_user
        else
          reject_unauthorized_connection
        end
      end
  end
end</pre> 

参考: https://stackoverflow.com/questions/38470463/rails-devise-action-cable


