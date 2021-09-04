---
id: 765
title: Railsで特定のバージョンのmigrationをロールバックする方法
date: 2019-07-09T16:52:34+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2019/07/09/763-revision-v1/
permalink: /?p=765
---
git branchを行ったり来たりしていてしっかり管理しないとlocalでゴミテーブルができるのでしっかりdownさせる。

`db:migrate:status`でマイグレーションの状態を確認。
 
<pre class="theme:dark-terminal lang:default decode:true " >bundle exec rake db:migrate:status</pre> 


最初の数列をコピペしてそのファイルだけ戻す。
 
<pre class="theme:dark-terminal lang:default decode:true " >bundle exec rake db:migrate:down VERSION=20190611235049 </pre> 

その後必要ならSQLでmigrationエントリを消す

 
<pre class="theme:dark-terminal lang:default decode:true " >rails dbconsole
> delete from schema_migrations where version = '20190611235049';  #sqlite3</pre> 
