---
id: 803
title: Mov to mp4/gif with ffmpeg
date: 2019-10-24T08:49:13+09:00
author: Shohei
layout: post
guid: https://www.wazalab.com/?p=803
permalink: /2019/10/24/ffmpeg_mov_to_mp4/
siteorigin_page_settings:
  - 'a:6:{s:6:"layout";s:7:"default";s:10:"page_title";b:1;s:15:"masthead_margin";b:1;s:13:"footer_margin";b:1;s:16:"display_masthead";b:1;s:22:"display_footer_widgets";b:1;}'
categories:
  - App
  - IT
  - Linux
tags:
  - animation gif
  - ffmpeg
  - mov
  - mp4
---
For the sake of my memory.

### mov to mp4


 
<pre class="theme:dark-terminal lang:default decode:true " >$ ffmpeg -i medium.MOV -vcodec h264 -acodec mp2 medium.mp4</pre> 



### mov to animation gif

 
<pre class="theme:dark-terminal lang:default decode:true " >$ ffmpeg -i medium.mov -r 24 medium.gif</pre> 

