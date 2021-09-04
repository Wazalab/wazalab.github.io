---
id: 219
title: macOSのelasticserachの使用メモリサイズの変更方法
date: 2017-06-21T11:10:25+09:00
author: Shohei
layout: revision
guid: http://www.wazalab.com/2017/06/21/218-revision-v1/
permalink: /?p=219
---
気づいたら、2G使ってたので変更。そもそも開発用なのでそんなに必要ない。

```
$ vi /usr/local/opt/elasticsearch/libexec/config/jvm.options

-Xms256m # from -Xms2g
-Xmx256m # from -Xmx2g
```

256Mも使ってないけど一応。