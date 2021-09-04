---
id: 952
title: 'sqlite3でカラムを削除する方法 [SQLite3::ConstraintException: FOREIGN KEY constraint failed: DROP TABLE &#8220;users&#8221;]'
date: 2021-08-04T17:53:01+09:00
author: Shohei
layout: post
guid: https://www.wazalab.com/?p=952
permalink: '/2021/08/04/sqlite3%e3%81%a7%e3%82%ab%e3%83%a9%e3%83%a0%e3%82%92%e5%89%8a%e9%99%a4%e3%81%99%e3%82%8b%e6%96%b9%e6%b3%95-sqlite3constraintexception-foreign-key-constraint-failed-drop-table-users/'
siteorigin_page_settings:
  - 'a:6:{s:6:"layout";s:7:"default";s:10:"page_title";b:1;s:15:"masthead_margin";b:1;s:13:"footer_margin";b:1;s:16:"display_masthead";b:1;s:22:"display_footer_widgets";b:1;}'
categories:
  - IT
  - Linux
  - Programming
tags:
  - rails
  - sqlite
---
Railsのマイグレーションでcolumnを追加後にrollbackすると下記のようなエラーができることがある。sqliteにはカラムを削除する方法がないので、一旦テーブルを消して作り直すのだが、外部キー制約のため削除できず、エラーになる。
<pre class="theme:dark-terminal lang:sh decode:true">== 20210802023517 AddJtiToUser: reverting =====================================
-- remove_index(:users, {:column=&gt;:jti})
   -&gt; 0.0022s
-- remove_column(:users, :jti, :string)
rake aborted!
StandardError: An error has occurred, this and all later migrations canceled:

SQLite3::ConstraintException: FOREIGN KEY constraint failed: DROP TABLE "users"


Caused by:
ActiveRecord::InvalidForeignKey: SQLite3::ConstraintException: FOREIGN KEY constraint failed: DROP TABLE "users"だ</pre>
だが、しかしsqlite3.35でカラム削除ができるようになったので、最新バージョンを使えば良い。手元のubuntuのsqlite3は、3.31だったので、ソースからコンパイルする。
<pre class="theme:dark-terminal lang:sh decode:true">$ wget https://www.sqlite.org/2021/sqlite-autoconf-3360000.tar.gz
$ tar zxvf sqlite-autoconf-3360000.tar.gz 
$ cd sqlite-autoconf-3360000/
$ ./configure 
$ make</pre>
でこのバージョンを使って、
<pre class="theme:dark-terminal lang:sh decode:true">$ sqlite-autoconf-3360000/sqlite3 ~/my_project/db/development.sqlite3

# delete index and then drop column

sqlite&gt; drop index index_users_on_jti;
sqlite&gt; ALTER TABLE users DROP COLUMN jti;

</pre>
その後、Railsの場合、migration version を消すとrollbackしたことになる。
<pre class="theme:dark-terminal lang:sh decode:true">$ rails db:migrate:status | tail -n 2
   up     20210802023517  Add jti to user

$ sqlite3 db/development.sqlite3
> delete from schema_migrations where version='[version_number]';</pre>
&nbsp;

&nbsp;