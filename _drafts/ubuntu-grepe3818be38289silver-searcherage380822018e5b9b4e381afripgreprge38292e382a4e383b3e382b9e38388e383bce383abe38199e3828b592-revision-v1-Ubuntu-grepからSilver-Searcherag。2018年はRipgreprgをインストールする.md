---
id: 597
title: '[Ubuntu] grepからSilver Searcher(ag)。2018年はRipgrep(rg)をインストールする'
date: 2018-09-23T09:07:20+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2018/09/23/592-revision-v1/
permalink: /?p=597
---
昔は、`find . | xargs grep 2>/dev/null`を使ってました。agに変更後、その速さに驚き。2018年はそれよりも早い[Ripgrep](https://github.com/BurntSushi/ripgrep)を採用しています。

Ubuntuでのインストールは、下記の通り。

<pre class="theme:dark-terminal lang:default decode:true " ># ripgrep
curl -LO https://github.com/BurntSushi/ripgrep/releases/download/0.8.1/ripgrep_0.8.1_amd64.deb
sudo dpkg -i ripgrep_0.8.1_amd64.deb &amp;&amp; rm ripgrep_0.8.1_amd64.deb</pre> 

使い方はシンプルで`rg 検索ワード`。

<pre class="theme:dark-terminal lang:default decode:true " >shohey1226@pro79.local:/Users/shohey1226/dotfiles&gt; rg vim
init.vim
1:set runtimepath^=~/.vim runtimepath+=~/.vim/after
3:source ~/.vimrc

starttmux.sh
10:  # create a new tmux session, starting vim from a saved session in the new window
13:  # Select pane 1, set dir to api, run vim
23:  tmux send-keys -t $session:1 "cd ~/git/Gauss; vim -S $HOME/.vim/sessions/gauss.vim" C-m
44:# return to main vim window

README.markdown
19:    $ git submodule init .vim/vundle.git/
20:    $ git submodule update .vim/vundle.git/</pre> 

