---
id: 579
title: '[Rails] Device利用時のconnection.rb(Action Cable)'
date: 2018-09-20T20:54:16+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2018/09/20/577-revision-v1/
permalink: /?p=579
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


