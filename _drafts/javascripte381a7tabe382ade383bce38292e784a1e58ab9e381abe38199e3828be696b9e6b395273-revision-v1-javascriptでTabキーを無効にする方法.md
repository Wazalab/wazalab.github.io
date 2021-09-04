---
id: 277
title: javascriptでTabキーを無効にする方法
date: 2018-01-20T09:24:29+09:00
author: Shohei
layout: revision
guid: http://www.wazalab.com/2018/01/20/273-revision-v1/
permalink: /?p=277
---
メモ。

```
window.onkeydown = function(e) {
  if (e.keyCode == 9)
    return false; // Disable Tab!
}
```