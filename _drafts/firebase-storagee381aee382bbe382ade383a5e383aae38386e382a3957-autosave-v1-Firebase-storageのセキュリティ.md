---
id: 959
title: Firebase storageのセキュリティ
date: 2021-08-26T14:19:09+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/?p=959
permalink: /?p=959
---
FirebaseのAPIを使ってアップロードした場合、

<blockquote><a href="https://firebasestorage.googleapis.com/v0/b/%3Cproject%3E.appspot.com/o/users%2F91b11d904a5f0eb398a334ssb5%2Fimages%2F60.jpg?alt=media&amp;token=5908a2d0-7630-4a35-8be5-e3cc8adbd723" target="_blank" rel="nofollow noopener">https://firebasestorage.googleapis.com/v0/b/&lt;project&gt;.appspot.com/o/users%2F91b11d904a5f0eb398a334ssb5%2Fimages%2F60.jpg?alt=media&amp;token=5908a2d0-7630-4a35-8be5-e3cc8adbd723</a></blockquote>

のようなtoken付きのlinkをgetDownloadLinkで取得できる。このトークンがあると、誰でもダンロードできてしまう。

ダウンロード対象のダウンロードオブジェクトがセキュリティに気を使わなくてよいなら、tokenごとDBに保存。そうでない場合は、ref(path)だけ保存しておいて、クライアント側で毎回getDownloadLinkするのが良さそうだ。