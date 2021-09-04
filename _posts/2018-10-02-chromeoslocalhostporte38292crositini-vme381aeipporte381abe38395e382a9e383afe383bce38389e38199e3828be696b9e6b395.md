---
id: 609
title: '[Chromeos]localhost:portをCrostini VMのIP:portにフォワードする方法'
date: 2018-10-02T20:44:29+09:00
author: Shohei
layout: post
guid: https://www.wazalab.com/?p=609
permalink: '/2018/10/02/chromeoslocalhostport%e3%82%92crositini-vm%e3%81%aeipport%e3%81%ab%e3%83%95%e3%82%a9%e3%83%af%e3%83%bc%e3%83%89%e3%81%99%e3%82%8b%e6%96%b9%e6%b3%95/'
siteorigin_page_settings:
  - 'a:6:{s:6:"layout";s:7:"default";s:10:"page_title";b:1;s:15:"masthead_margin";b:1;s:13:"footer_margin";b:1;s:16:"display_masthead";b:1;s:22:"display_footer_widgets";b:1;}'
categories:
  - IT
  - Linux
  - Web
tags:
  - chromeos
  - crostini
---
Crostiniがstableにきた！と思ってbetaからstableに変更したらLinuxVMが無くなりました。気をつけてください。。
stableに変更したのが理由なのかわかりませんが、socatでcrostiniのlocalhostのポートへのマッピングがうまくいかなくなりました。
例えば、開発時にlocalhost:3000をcrostini　vm上で立ててもホストのchromeosのchromeからアクセスできなくなりました。

下記のようなエラーでうまくいきません。

```
This site can’t be reached
localhost refused to connect.

ERR_CONNECTION_REFUSED
```

解決方法をさがしていると[Reddit](https://www.reddit.com/r/Crostini/comments/8o0hg9/port_forwarding_in_the_works/)で同じようなことを聞いている人がいました。

[Connection Fowarderd](https://chrome.google.com/webstore/detail/connection-forwarder/ahaijnonphgkgnkbklchdhclailflinn)というextensionを入れ、forwardingを設定します。

<img src="https://www.wazalab.com/wp-content/uploads/2018/10/スクリーンショット-2018-10-02-20.41.51.png" alt="" width="391" height="442" class="alignnone size-full wp-image-610" />

ifconfigでVMのIPを調べてDestinationを適宜変更してください。

