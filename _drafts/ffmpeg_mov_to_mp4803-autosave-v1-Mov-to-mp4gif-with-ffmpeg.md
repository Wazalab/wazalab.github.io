---
id: 807
title: Mov to mp4/gif with ffmpeg
date: 2019-10-24T08:56:24+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2019/10/24/803-autosave-v1/
permalink: /?p=807
---
For the sake of my memory.

<h3>mov to mp4</h3>

$ ffmpeg -i medium.MOV -vcodec h264 -acodec mp2 medium.mp4

<h3>mov to animation gif</h3>

<pre><code>$ ffmpeg -i medium.mov -r 24 medium.gif
</code></pre>