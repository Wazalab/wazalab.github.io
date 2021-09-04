---
id: 941
title: Railsで一番シンプルな検索の実装
date: 2021-07-27T17:43:24+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/?p=941
permalink: /?p=941
---
数分できたので多分最高にシンプル。

コントローラにクラスメソッド追加。
<pre class="theme:sublime-text lang:ruby decode:true" title="controller#index">def index
  @commands = Pipe.search(params[:q]).paginate(page: params[:page])
end</pre>
モデルにロジック記述。ポイントはCase insensitiveにしているところとOR検索してるところ。
<pre class="theme:sublime-text lang:ruby decode:true">  def self.search(query)
    if query.present?
      q = query.downcase
      Pipe.where('lower(title) LIKE ?', "%#{q}%").or(Pipe.where('lower(line) LIKE ?', "%#{q}%"))
    else
      Pipe.all
    end
  end</pre>
Viewはこんな感じ。
<pre class="theme:sublime-text lang:ruby decode:true ">&lt;%= form_with(url: '/cmds', method: 'get', local: true) do %&gt;
  &lt;%= text_field_tag(:q, "", class: "uk-input uk-form-width-medium") %&gt;
　　&lt;%= submit_tag("Search", class: "uk-button uk-button-default") %&gt;
&lt;% end %&gt;</pre>
&nbsp;