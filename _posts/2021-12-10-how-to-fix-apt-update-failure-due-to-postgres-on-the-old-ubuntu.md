---
layout: post
title: "apt updateができない(postgresのところで止まる)ときの対処法"
date: 2021-12-10 06:34 +0000
categories:
  - IT
  - Linux
tags:  
  - ubuntu
---

Ubuntu14のサーバがあり、Certbotが下記のエラーで更新できないでいた。

```shell
Plugins selected: Authenticator nginx, Installer nginx                                                                                              
Attempting to renew cert (bi.gsacademy.com) from /etc/letsencrypt/renewal/bi.gsacademy.com.conf produced an unexpected error: ("bad handshake: Error
([('SSL routines', 'SSL3_GET_SERVER_CERTIFICATE', 'certificate verify failed')],)",). Skipping.                                                     
All renewal attempts failed. The following certs could not be renewed:                                                                              
  /etc/letsencrypt/live/bi.gsacademy.com/fullchain.pem (failure)                                                                                                                                                       
```

単純にバージョンが古いと思い、`apt update`だがしかし、Postgresのところで止まる。。

```shell
Err http://apt.postgresql.org trusty-pgdg/main amd64 Packages                                                                                       
  404  Not Found [IP: 147.75.85.69 80]                                                                                                              
Ign http://apt.postgresql.org trusty-pgdg/main Translation-en_US                                                                                    
Ign http://apt.postgresql.org trusty-pgdg/main Translation-en                                                                                       
W: Failed to fetch http://apt.postgresql.org/pub/repos/apt/dists/trusty-pgdg/main/binary-amd64/Packages  404  Not Found [IP: 147.75.85.69 80]  
```

古いのを使っているのは知っている。これを消す方法をググって、格闘すること30分。 /etc/apt/sources.list.d/pgdg.listを変更する方法でスキップできた。

```
# vi /etc/apt/sources.list.d/pgdg.list
# comment out the line 
# deb http://apt.postgresql.org/pub/repos/apt/ trusty-pgdg main

# then,
# apt update ## works!
```

certbotも`apt upgrade certbot`したら動くようになった。
