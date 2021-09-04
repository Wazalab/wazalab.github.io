---
id: 215
title: Sublime Text 3のSwitch Projectの編集方法・削除方法
date: 2017-06-13T08:16:48+09:00
author: Shohei
layout: post
guid: http://www.wazalab.com/?p=215
permalink: '/2017/06/13/sublime-text-3%e3%81%aeswitch-project%e3%81%ae%e7%b7%a8%e9%9b%86%e6%96%b9%e6%b3%95%e3%83%bb%e5%89%8a%e9%99%a4%e6%96%b9%e6%b3%95/'
categories:
  - IT
tags:
  - st
  - sublime text
  - sublime text 3
---
消したはずのプロジェクトが残る現象発生。
削除するのは現セッションのRecent workspacesから取り除く必要あり。

Sublime Textを閉じたあとに

```
$ vi ~/Library/Application\ Support/Sublime\ Text\ 3/Local/Session.sublime_session
```

で、recent workspacesを消す。

```
    "workspaces":
    {
        "recent_workspaces":
        [
            "/Users/shohey1226/gdrive/st_projects/Gauss.sublime-workspace",
            "/Users/shohey1226/gdrive/st_projects/Personal.sublime-workspace"　 
        ]
    }
```