---
id: 808
title: Mov to mp4/gif with ffmpeg
date: 2019-10-24T09:00:53+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2019/10/24/803-revision-v1/
permalink: /?p=808
---
For the sake of my memory.

### mov to mp4


 
<pre class="theme:dark-terminal lang:default decode:true " >$ ffmpeg -i medium.MOV -vcodec h264 -acodec mp2 medium.mp4</pre> 



### mov to animation gif

 
<pre class="theme:dark-terminal lang:default decode:true " >$ ffmpeg -i medium.mov -r 24 medium.gif</pre> 

