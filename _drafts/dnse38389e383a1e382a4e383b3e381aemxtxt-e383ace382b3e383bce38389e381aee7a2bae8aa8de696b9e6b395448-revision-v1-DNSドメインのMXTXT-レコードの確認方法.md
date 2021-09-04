---
id: 449
title: '[DNS]ドメインのMX,TXT レコードの確認方法'
date: 2018-09-09T06:51:10+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2018/09/09/448-revision-v1/
permalink: /?p=449
---
mxやtxtレコードのバックアップを撮りたいときがあります。 `nslookup -q=any`でとってこれます。　cnameなどはFQDNを知らないと取れないので漏れなく全レコードをバックアップをとるのは難しい？

 
<pre class="lang:sh decode:true " >$ nslookup -q=any gsacademy.com
;; Truncated, retrying in TCP mode.
Server:         100.115.92.193
Address:        100.115.92.193#53

Non-authoritative answer:
Name:   gsacademy.com
Address: 50.63.202.10
gsacademy.com   nameserver = ns31.domaincontrol.com.
gsacademy.com   nameserver = ns32.domaincontrol.com.
gsacademy.com
        origin = ns31.domaincontrol.com
        mail addr = dns.jomax.net
        serial = 2018082202
        refresh = 28800
        retry = 7200
        expire = 604800
        minimum = 600
gsacademy.com   mail exchanger = 1 ASPMX.L.GOOGLE.com.
gsacademy.com   mail exchanger = 5 ALT1.ASPMX.L.GOOGLE.com.
gsacademy.com   mail exchanger = 5 ALT2.ASPMX.L.GOOGLE.com.
gsacademy.com   mail exchanger = 10 ALT3.ASPMX.L.GOOGLE.com.
gsacademy.com   mail exchanger = 10 ALT4.ASPMX.L.GOOGLE.com.
gsacademy.com   mail exchanger = 10 ASPMX.L.GOOGLE.com.
gsacademy.com   mail exchanger = 20 ALT1.ASPMX.L.GOOGLE.com.
gsacademy.com   mail exchanger = 30 ALT2.ASPMX.L.GOOGLE.com.
gsacademy.com   mail exchanger = 40 ASPMX2.GOOGLEMAIL.com.
gsacademy.com   mail exchanger = 50 ASPMX3.GOOGLEMAIL.com.
gsacademy.com   text = "google-site-verification=gOpqy_QueJfWIXWxVbv81GdIIRGM2zcPj5tU7UTT0gw"
gsacademy.com   text = "google-site-verification=f6_DPrzy2Gs3k-VJBwBaIcRKAuozIp_RQJA1hO71CTY"
gsacademy.com   text = "google-site-verification=FDLsQHtXCoZFqWgN7NfAtUhGJmluPYkJdc0iSi7jX6Y"
</pre> 

