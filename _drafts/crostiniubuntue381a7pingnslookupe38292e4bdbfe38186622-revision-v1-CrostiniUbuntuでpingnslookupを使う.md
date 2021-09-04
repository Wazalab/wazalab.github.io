---
id: 623
title: Crostini(Ubuntu)でping/nslookupを使う
date: 2018-10-10T21:59:24+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2018/10/10/622-revision-v1/
permalink: /?p=623
---
Crostiniというより、Ubuntuなのですが、nslookupがなくて名前解決できなくて辛い。
hostコマンドを使うところなんだろうけど、気づけばnslookupを打ってるのでやはり欲しい。

`dnsutils`というパッケージに入っているので、これを入れるだけ。

あとpingは普通に使うでしょってことでこれもapt-getで。

 
<pre class="theme:dark-terminal lang:default decode:true " >$ sudo apt-get install dnsutils
$ sudo apt-get install iputils-ping
</pre> 
