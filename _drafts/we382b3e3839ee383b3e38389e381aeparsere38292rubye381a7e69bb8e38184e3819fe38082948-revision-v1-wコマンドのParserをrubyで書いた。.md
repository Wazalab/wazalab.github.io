---
id: 949
title: wコマンドのParserをrubyで書いた。
date: 2021-08-03T23:20:02+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/?p=949
permalink: /?p=949
---
/dev/pts以下にファイルがない時があったのでwコマンドをパースすることにした。（おそらくLXDコンテナ内だからかな）
<pre class="theme:sublime-text lang:ruby decode:true ">  def idle_by_w_command
    cmd = "w | grep #{self.username} | awk '{print $5}'"
    output = self._exec(cmd, "admin")
    output.split("\n").map{|line|
      if line =~ /(\d+)days/
        $1.to_i * 3600 * 24
      elsif line =~ /(\d+):(\d+)m$/
        $1.to_i * 3600 + $2.to_i * 60
      elsif line =~ /(\d+):(\d+)$/
        $1.to_i * 60 + $2.to_i 
      elsif line =~ /([0-9]*[.])?[0-9]+s$/
        $1.to_i
      end
    }.compact
  end</pre>