---
id: 537
title: vim-session プラグインを使ってtabごとsessionを保存する方法
date: 2018-09-15T17:21:09+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2018/09/15/529-revision-v1/
permalink: /?p=537
---
[tmuxで指定したwindow/paneから始める方法 [再起動も安心]](https://www.wazalab.com/2018/09/15/tmux%E3%81%A7%E6%8C%87%E5%AE%9A%E3%81%97%E3%81%9Fwindowpane%E3%81%8B%E3%82%89%E5%A7%8B%E3%82%81%E3%82%8B%E6%96%B9%E6%B3%95/)を説明したが、Vimのセッション情報もタブを含めてRestoreしたい。 最初は全て自動で保存しようとしたが、必要に応じて自分でセッションを保存した方が実用的。ブラウザのタブのように、使わないタブが残ってしまい必要のないタブが溜まっていってしまう。

vimはデフォルトでセッションの管理が可能だが、細かいところまで残すには[vim-session](https://github.com/xolox/vim-session)を使った方が良い。

#### .vimrc
 
<pre class="lang:vim decode:true " >" session
call dein#add('xolox/vim-misc')
call dein#add('xolox/vim-session')

let g:session_autosave = 'no'
let g:session_autoload = 'no'
nnoremap &lt;Leader&gt;ss :SaveSession
</pre> 

SaveSessionとタイプするのが面倒なのでノーマルモードで`\ss`でSaveSessionを呼び出す。


例えば、セッションをProject毎に保存する場合は、引数のセッション名にプロジェクト名を指定することでプロジェクトごとのセッションの作成が可能。

 
<pre class="lang:vim decode:true " >: SaveSession &lt;session name&gt;</pre> 

 で`.vim/sessions/<session name>.vim`に保存されるのでこれをvimの引数として呼び出せばよい。


 
<pre class="lang:sh decode:true " >$ vim -S /home/shohey1226/.vim/sessions/gauss.vim
</pre> 

実際は、tmuxの起動スクリプトに書いておけば良い。

 
<pre class="lang:sh decode:true " >tmux send-keys -t $session:1 "cd ~/git/Gauss; vim -S $HOME/.vim/sessions/gauss.vim" C-m</pre> 
