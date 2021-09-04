---
id: 208
title: '[React Native]日本語環境下で日本語が垂直のセンターを取れない問題の対処法'
date: 2017-05-30T09:58:05+09:00
author: Shohei
layout: revision
guid: http://www.wazalab.com/2017/05/30/204-revision-v1/
permalink: /?p=208
---
地味に辛いこの問題。iOSが英語環境（設定 -> 一般 -> 言語と地域が英語）の場合は問題なくセンターとれるのですが、日本語環境にすると`flex-start`みたいにpaddingTopがゼロになってしまう問題があります。やっかいなのは、paddingで調整するとAndroidはこの問題がないため表示がずれます。。

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Text(iOS) with Japanese can&#39;t be vertically center&#39;ed on <a href="https://twitter.com/hashtag/reactnative?src=hash">#reactnative</a>  I need to add padding on top which is not good for android. :S <a href="https://t.co/IjRHwK6NjA">pic.twitter.com/IjRHwK6NjA</a></p>&mdash; Shohei Kameda (@shohey1226) <a href="https://twitter.com/shohey1226/status/869334118844178432">May 29, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

コードはボタンだったら、下記みたいな感じ。ボタンに限らず、この問題はおきていると思います。


 
<pre class="lang:js decode:true " >&lt;FAIcon.Button onPress={this.onPressFBLogin} name="facebook" iconStyle={{paddingRight:5, paddingLeft: 10}} backgroundColor="#3b5998" borderRadius={0}&gt;
	&lt;Text style={{fontFamily: 'Arial', fontSize: 14, color: 'white'}}&gt;Facebookでログイン・登録&lt;/Text&gt;
&lt;/FAIcon.Button&gt;</pre> 



解決方法は、fontサイズと同じViewでラップする。（試行錯誤後、閃きました）


 
<pre class="lang:js decode:true " >&lt;FAIcon.Button onPress={this.onPressFBLogin} name="facebook" iconStyle={{paddingRight:5, paddingLeft: 10}} backgroundColor="#3b5998" borderRadius={0}&gt;
	&lt;View style={{height: 14, margin:5}}&gt;&lt;Text style={{fontFamily: 'Arial', fontSize: 14, color: 'white'}}&gt;Facebookでログイン・登録&lt;/Text&gt;&lt;/View&gt;
&lt;/FAIcon.Button&gt;
</pre> 



<img src="http://www.wazalab.com/wp-content/uploads/2017/05/スクリーンショット-2017-05-30-8.45.58-300x61.png" alt="" width="300" height="61" class="alignnone size-medium wp-image-205" />
