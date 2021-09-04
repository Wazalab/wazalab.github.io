---
id: 217
title: Sublime Text 3のSwitch Projectの編集方法・削除方法
date: 2017-06-13T08:16:53+09:00
author: Shohei
layout: revision
guid: http://www.wazalab.com/2017/06/13/215-autosave-v1/
permalink: /?p=217
---
消したはずのプロジェクトが残る現象発生。
削除するのは現セッションのRecent workspacesから取り除く必要あり。

Sublime Textを閉じたあとに

<pre><code>$ vi ~/Library/Application\ Support/Sublime\ Text\ 3/Local/Session.sublime_session
</code></pre>

で、recent workspacesを消す。

<pre><code>    "workspaces":
    {
        "recent_workspaces":
        [
            "/Users/shohey1226/gdrive/st_projects/Gauss.sublime-workspace",
            "/Users/shohey1226/gdrive/st_projects/Personal.sublime-workspace"　 
        ]
    }
</code></pre>