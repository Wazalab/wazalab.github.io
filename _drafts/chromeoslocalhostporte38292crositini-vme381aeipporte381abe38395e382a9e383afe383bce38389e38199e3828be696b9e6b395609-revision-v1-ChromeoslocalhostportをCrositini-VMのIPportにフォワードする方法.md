---
id: 612
title: '[Chromeos]localhost:portをCrositini VMのIP:portにフォワードする方法'
date: 2018-10-02T20:44:29+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2018/10/02/609-revision-v1/
permalink: /?p=612
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

