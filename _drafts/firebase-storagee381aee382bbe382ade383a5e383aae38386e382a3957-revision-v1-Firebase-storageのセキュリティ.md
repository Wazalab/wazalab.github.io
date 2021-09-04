---
id: 960
title: Firebase storageのセキュリティ
date: 2021-08-26T14:19:26+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/?p=960
permalink: /?p=960
---
FirebaseのAPIを使ってアップロードした場合、
<blockquote>https://firebasestorage.googleapis.com/v0/b/&lt;project&gt;.appspot.com/o/users%2F91b11d904a5f0eb398a334ssb5%2Fimages%2F60.jpg?alt=media&amp;token=5908a2d0-7630-4a35-8be5-e3cc8adbd723</blockquote>
のようなtoken付きのlinkをgetDownloadLinkで取得できる。このトークンがあると、誰でもダンロードできてしまう。

ダウンロード対象のダウンロードオブジェクトがセキュリティに気を使わなくてよいなら、tokenごとDBに保存。そうでない場合は、ref(path)だけ保存しておいて、クライアント側で毎回getDownloadLinkするのが良さそうだ。