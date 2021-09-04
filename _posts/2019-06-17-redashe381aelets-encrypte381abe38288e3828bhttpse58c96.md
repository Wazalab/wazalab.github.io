---
id: 737
title: 'RedashのLet&#8217;s encryptによるhttps化'
date: 2019-06-17T09:23:59+09:00
author: Shohei
layout: post
guid: https://www.wazalab.com/?p=737
permalink: '/2019/06/17/redash%e3%81%aelets-encrypt%e3%81%ab%e3%82%88%e3%82%8bhttps%e5%8c%96/'
siteorigin_page_settings:
  - 'a:6:{s:6:"layout";s:7:"default";s:10:"page_title";b:1;s:15:"masthead_margin";b:1;s:13:"footer_margin";b:1;s:16:"display_masthead";b:1;s:22:"display_footer_widgets";b:1;}'
categories:
  - IT
  - Linux
  - Service
tags:
  - letsencrypt
  - nginx
  - redash
---
Business Intelligenceの[Redash](https://redash.io/)はデータから経営判断するとても良いツールです。
SQLさえ書ければ、難しいプログラミング知識も必要のないのでテックに強いスタッフにSQLを勉強してもらえれば使ってもらえます。

AWSのAMIイメージも用意されてるのですぐに導入できます。しかし、TLS/SSL化されてないので大事なデータを傍受されてしまう危険性があります。
そこで、https化ですが、少し手間取ったので記録しておきます。

AMIを中身を見るとわかるのですが、DockerでRedash周りのプロセスが動いています。なので、方針として、ホスト側にlet's encryptを入れる。Let's encryptのキーがあるディレクトリをdockerからマウント。ホストでcertbotのリフレッシュで期限切れになるのを防ぐという感じです。

### 1. ホスト側にcertbot導入

方法はいろいろあると思いますが、AWSのRoute53を利用して作成しました。

参考：https://qiita.com/komazarari/items/88c1ed18765fb7cab6c1

https://certbot.eff.org/lets-encrypt/ubuntubionic-other通りにcertbotをインストールして、
 
<pre class="theme:dark-terminal lang:default decode:true " >$ sudo apt-get update
$ sudo apt-get install software-properties-common
$ sudo add-apt-repository universe
$ sudo add-apt-repository ppa:certbot/certbot
$ sudo apt-get update
$ sudo apt-get install certbot </pre> 

pipでcertbot-dns-route53アドオンをインストール

<pre class="theme:dark-terminal lang:default decode:true " >$ sudo apt install python3-pip libssl-dev libffi6
$ sudo pip3 install certbot-dns-route53
</pre> 

AWSのsecretkey/apikeyをセットして、下記で/etc/letsencrypt以下にファイルを作成。systemd経由で期限切れの確認が走っているみたい。

 
<pre class="theme:dark-terminal lang:default decode:true " > sudo certbot certonly --dns-route53 \
    -d 'redash.exmpale.com' \
    --email redashadmin@example.com \
    -n \
    --agree-tos \
</pre> 


### 2. Redash nginxのconfig作成

参考：https://gist.github.com/arikfr/64c9ff8d2f2b703d4e44fe9e45a7730e

redashのインストール場所に行って、ディレクトリとファイルを作成。
 
<pre class="theme:dark-terminal lang:default decode:true " >$ sudo su
$ mkdir /opt/redash/nginx
$ vi /opt/redash/nginx/nginx.conf
</pre> 

nginx.confは下記のようにした。
 
<pre class="theme:dark-terminal lang:default decode:true " >upstream redash {
  server redash:5000;
}

server {
    listen      80;
    listen [::]:80;
    server_name redash.example.com;

    location ^~ /ping {
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $http_x_forwarded_proto;

        proxy_pass       http://redash;
    }

    location / {
        rewrite ^ https://$host$request_uri? permanent;
    }

    location ^~ /.well-known {
        allow all;
        root  /data/letsencrypt/;
    }
}

server {
    listen      443           ssl http2;
    listen [::]:443           ssl http2;
    server_name               redash.example.com;

    add_header                Strict-Transport-Security "max-age=31536000" always;

    ssl_session_cache         shared:SSL:20m;
    ssl_session_timeout       10m;

    ssl_protocols             TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    ssl_ciphers               "ECDH+AESGCM:ECDH+AES256:ECDH+AES128:!ADH:!AECDH:!MD5;";

    ssl_stapling              on;
    ssl_stapling_verify       on;
    resolver                  8.8.8.8 8.8.4.4;

    ssl_certificate           /etc/letsencrypt/live/redash.example.com/fullchain.pem;
    ssl_certificate_key       /etc/letsencrypt/live/redash.example.com/privkey.pem;
    ssl_trusted_certificate   /etc/letsencrypt/live/redash.example.com/chain.pem;

    access_log                /dev/stdout;
    error_log                 /dev/stderr info;

    # other configs

    location / {
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $http_x_forwarded_proto;

        proxy_pass       http://redash;
    }
}</pre> 


### 3. Redashのdocker composeファイルの編集

/opt/redash/docker-compose.ymlを開いて、

 
<pre class="theme:dark-terminal lang:default decode:true " >nginx:
 image: nginx:latest
 ports:
   - "80:80"
   - "443:443"
 depends_on:
   - server
 links:
   - server:redash
 volumes:
   - /opt/redash/nginx/nginx.conf:/etc/nginx/conf.d/default.conf
   - /etc/letsencrypt:/etc/letsencrypt
 restart: always
</pre> 

その後、リスタート。
 
<pre class="theme:dark-terminal lang:default decode:true " >docker-compose up -d</pre> 

### systemdのpost-hook設定

直接certbot.serviceファイルを触るのはあまり良いプラクティスでなさそうだが、、

 
<pre class="theme:dark-terminal lang:default decode:true " >$ sudo vi /lib/systemd/system/certbot.service</pre> 


で開いて下記を追加(/usr/local/bin/certbotに変更。でないとバージョンのエラーで失敗していた)

 
<pre class="theme:dark-terminal lang:default decode:true " >ExecStart=/usr/local/bin/certbot -q renew --post-hook "docker-compuse up -d"</pre> 

sysytemdのコンフィグをリロードすればOK.

 
<pre class="theme:dark-terminal lang:default decode:true " >systemctl daemon-reload</pre> 

