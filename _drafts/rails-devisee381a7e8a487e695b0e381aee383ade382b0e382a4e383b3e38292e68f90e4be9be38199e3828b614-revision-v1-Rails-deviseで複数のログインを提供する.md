---
id: 621
title: '[Rails] deviseで複数のログインを提供する'
date: 2018-10-09T18:45:17+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2018/10/09/614-revision-v1/
permalink: /?p=621
---
サービスを運用していると現在のUIを変更するだけでなく、別途違うUIを用意する必要がでてきます。
A/Bテストでもそうですし、単純に現行UIを残しつつ新しいものを用意する場合です。この場合、単純にRouteの変更だけで復旧できるので安全でもあります。

今回はdeviseを用いたログインUIを複数作るケースです。deviseは魔法感が高いライブラリですが、基本的にオーバーロードして適宜変更で乗り切れるように思います。

まずは、sessionを継承します。 action_v2というActionを作りました。基本的にdeviseのソースをコピペです。ハマりどころはfilterをちゃんと設定するところでしょうか。
`prepend_before_action`ですね。create_v2を入れないとメソッドが走りません。　また、create_v2で　auth_optionsのrecallにnew_v2を設定することでvalidation失敗時にnew_v2にrouteされるようになります。


 
<pre class="lang:ruby decode:true " >class Students::SessionsController &lt; Devise::SessionsController 
  prepend_before_action :allow_params_authentication!, only: [ :create, :create_v2 ]

  def new_v2
    self.resource = resource_class.new(sign_in_params)
    clean_up_passwords(resource)
    yield resource if block_given?
    respond_with(resource, serialize_options(resource))
  end
  
  def create_v2 
    auth_options = { scope: :student, recall: "students/sessions#new_v2" }  
    self.resource = warden.authenticate!(auth_options)
    set_flash_message!(:notice, :signed_in)
    sign_in(resource_name, resource)
    yield resource if block_given?
    respond_with resource, location: after_sign_in_path_for(resource)
  end</pre> 

で、routeの設定は、下記のようになります。`as:`は両方必要なく片方につければ良いようです。

<pre class="lang:ruby decode:true " >    get "/student/login" =&gt; "students/sessions#new_v2", as: :student_login
    post "/student/login" =&gt; "students/sessions#create_v2"
</pre> 

最後に`new_v2.html.erb` の`fom_for`の`url:`に新しいパスを指定すればできあがり。

 
<pre class="lang:ruby decode:true " >&lt;%= form_for(resource, as: resource_name, url: student_login_path, html: {class: ""}) do |f| %&gt;</pre> 
