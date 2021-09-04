---
id: 599
title: 2018年のVimフィイル検索はfzfとripgrep(rg)
date: 2018-09-23T09:10:29+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2018/09/23/598-revision-v1/
permalink: /?p=599
---
sublime2 -> atom -> vscode -> sublime3 を経て5年ぶりぐらいにVimに帰ってきたわけですが、当時から色々変わっていて軽い浦島太郎状態です。ともかく大変なのはプロジェクト内のファイル検索と文字列検索。一度、sublimeのようなテキストサーチを経験してしまうとこれなしには開発できなくなってしまいました。

deinをパッケージのインストールに選び、deniteを試すも大きなプロジェクトではその遅さに困り、ctrlPとfzf両方試し、[fzf](https://github.com/junegunn/fzf)を利用することに決めました。決め手はその速さ。Blazing fastというやつです。

deinを利用しているので、.vimrcに`junegunn/fzf`と`junegunn/fzf.vim`を追加。

 
<pre class="lang:vim decode:true " >if dein#load_state($HOME . '/.cache/dein')
  call dein#begin($HOME . '/.cache/dein')
  ..
  call dein#add('junegunn/fzf', { 'build': './install --all', 'merged': 0 })
  call dein#add('junegunn/fzf.vim', { 'depends': 'fzf' })
  ..</pre> 


あとはショートカットを追加。ctrl-Pでファイル検索。ctrl-gで文字列検索です。私はバッファ検索はタブを利用しているのであまり使わないと思っていますが、一応追加。`\\`でvimコマンド一覧を表示できます。

 
<pre class="lang:vim decode:true " >" FZF
fun! FzfOmniFiles()
  let is_git = system('git status')
  if v:shell_error
    :Files
  else
    :GitFiles
  endif
endfun

nnoremap &lt;C-b&gt; :Buffers&lt;CR&gt;
nnoremap &lt;C-g&gt; :Rg&lt;Space&gt;
nnoremap &lt;leader&gt;&lt;leader&gt; :Commands&lt;CR&gt;
nnoremap &lt;C-p&gt; :call FzfOmniFiles()&lt;CR&gt;</pre> 


私はタブを利用してて、かつ私のtmuxのprefixキーのctl-tが被って利用できなそうだったので、ctrl-oで新しいタブを開くに変更。

let g:fzf_action = {
\ 'ctrl-o': 'tab split'
\ }


### 文字列検索はripgrep(rg)を採用

[ripgrepをインストール](https://www.wazalab.com/2018/09/23/ubuntu-grep%E3%81%8B%E3%82%89silver-searcherag%E3%80%822018%E5%B9%B4%E3%81%AFripgreprg%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%99%E3%82%8B/)して、Rgコマンドにpreviewを追加。ctrl-gで:Rgが出て、検索する文字列を入れて、Enter。それから`?`を押すとpreviewモードスタート。ファイルが右側に表示されて便利の極みです。


command! -bang -nargs=* Rg
\ call fzf#vim#grep(
\ 'rg --column --line-number --hidden --ignore-case --no-heading --color=always '.shellescape(<q-args>), 1,
\ <bang>0 ? fzf#vim#with_preview({'options': '--delimiter : --nth 4..'}, 'up:60%')
\ : fzf#vim#with_preview({'options': '--delimiter : --nth 4..'}, 'right:50%:hidden', '?'),
\ <bang>0)

