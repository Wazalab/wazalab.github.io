---
id: 531
title: 'tmuxで指定したwindow/paneから始める方法 [再起動も安心]'
date: 2018-09-15T17:11:59+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2018/09/15/515-revision-v1/
permalink: /?p=531
---
PCの再起動後にTmuxのwindowやpaneを自動的にある所定のものにしたい。[tmux-resurrect](https://github.com/tmux-plugins/tmux-resurrect)と[tmux-continuum](https://github.com/tmux-plugins/tmux-continuum)を試した試したがうまい具合にシャットダウン前の状態にしてくれないケースがあったので、それなら常に同じ状態からスタートした方が効率が良い気がしてきたので、そのような運用にしてみる。

tmuxのwindow作成などはtmuxコマンドから呼べるので設定ファイルを書くことができる。starttmux.shというスクリプトを作ってこれを.bashrcから呼べば良い。



 
<pre class="lang:sh decode:true " >session="work"

tmux has-session -t $session 2>/dev/null
if [ "$?" -eq 1 ] ; then
  # set up tmux
  tmux start-server

  # create a new tmux session, starting vim from a saved session in the new window
  tmux new-session -d -s $session

  # Select pane 1, set dir to api, run vim
  tmux selectp -t 1
  tmux send-keys "cd ~/git/Gauss; rails s" C-m

  # Split pane 1 horizontal by 65%, start redis-server
  tmux splitw -v -p 50
  tmux send-keys "cd ~/git/Gauss; rails c" C-m

  # create a new windows
  tmux new-window -t $session:1
  tmux send-keys -t $session:1 "cd ~/git/Gauss; vim -S $HOME/.vim/sessions/gauss.vim" C-m

  tmux new-window -t $session:2
  tmux send-keys -t $session:2 "cd ~/git/Gauss" C-m

else
  echo "session found. connecting..."
fi


# return to main vim window
tmux select-window -t $session:2

# Finished setup, attach to the tmux session!
tmux attach-session -t $session</pre> 
_*`C-m`はtmuxでの改行_


でこれを.bashrcの末尾追加すれれば毎回同じtmuxからスタートできる。

<pre class="lang:sh decode:true " ># attach automatically when it's opened
[ -z "$TMUX" ] &amp;&amp; . "$HOME/starttmux.sh"</pre> 


