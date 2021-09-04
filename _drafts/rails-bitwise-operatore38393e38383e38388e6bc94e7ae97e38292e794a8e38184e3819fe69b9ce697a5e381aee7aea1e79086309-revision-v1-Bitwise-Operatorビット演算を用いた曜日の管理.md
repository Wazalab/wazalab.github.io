---
id: 318
title: Bitwise Operator(ビット演算)を用いた曜日の管理
date: 2018-08-20T21:28:43+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2018/08/20/309-revision-v1/
permalink: /?p=318
---
Cronのように曜日を指定してジョブを発動するスケジューラを作成していて、外部にこれを保持するテーブルを作るのはセンスがないし、mondayからsundayのカラムを作るのも無駄に感じたので、ビットを使って曜日を表現し、これを用いて曜日の判別をしようと思いました。　[Railsでビット演算を用いたチェックボックスのフラグ管理をしてみた](https://techandlife.jp/posts/rails-chbox-flg-bit)を多いに参考にさせていただきました。

具体的には下記のように表現します。

* 月曜日 0b1
* 火曜日 0b10 
* 水曜日 0b100 
* 木曜日 0b1000 
* 金曜日 0b10000 
* 土曜日 0b100000 
* 日曜日 0b1000000 

`1100101`の場合、月曜日、水曜日、土曜日、日曜日が選択されている状態です。 1バイト以下の7bitで表現できるのでメモリにもストレージにも優しいですね。

Migrationファイルを作り、DBカラムwday作成。

<pre class="theme:monokai lang:sh decode:true " >$ rails g migration AddWdayToTimer wday:integer
$ vi db/migrate/20180818*
# limitを追加し、最小限1バイトのtinyint（dbによりますが)を作成
 t.integer :wday, limit: 1
</pre> 

View/Controllerは下記のようになりDBには整数で保存されます。

<pre class="theme:monokai lang:ruby decode:true">  &lt;div class="field"&gt;
  &lt;% %w(Mon Tue Wed Thu Fri Sat Sun).each_with_index do |v, i| %&gt;
    &lt;%= form.label :wday  do %&gt;
      &lt;%= form.check_box :wday, {multiple: true, checked: !form.object.wday.nil? &amp;&amp; (form.object.wday &amp; 2**i != 0)}, 2**i %&gt;
      &lt;%= v %&gt;
      &amp;nbsp
    &lt;% end %&gt;
  &lt;% end %&gt;
  &lt;/div&gt;</pre>


 
<pre class="theme:monokai lang:ruby decode:true " >  def create
    @timer = Timer.new(timer_params)
    @timer.wday = timer_params[:wday].map(&amp;:to_i).inject(&amp;:+)</pre> 


これを使う時に、`&`し判定します。　0でない場合は、曜日フラグが立っているということです。　
 
<pre class="theme:monokai lang:ruby decode:true " >[2] pry(main)&gt; Timer.last.wday
  Timer Load (0.2ms)  SELECT  "timers".* FROM "timers" ORDER BY "timers"."id" DESC LIMIT ?  [["LIMIT", 1]]
=&gt; 21
[3] pry(main)&gt; Timer.last.wday &amp; 0b1 #月曜日
  Timer Load (0.1ms)  SELECT  "timers".* FROM "timers" ORDER BY "timers"."id" DESC LIMIT ?  [["LIMIT", 1]]
=&gt; 1
[4] pry(main)&gt; Timer.last.wday &amp; 0b10 #火曜日
  Timer Load (0.1ms)  SELECT  "timers".* FROM "timers" ORDER BY "timers"."id" DESC LIMIT ?  [["LIMIT", 1]]
=&gt; 0
[5] pry(main)&gt; Timer.last.wday &amp; 0b100 #水曜日
  Timer Load (0.1ms)  SELECT  "timers".* FROM "timers" ORDER BY "timers"."id" DESC LIMIT ?  [["LIMIT", 1]]
=&gt; 4
</pre> 


