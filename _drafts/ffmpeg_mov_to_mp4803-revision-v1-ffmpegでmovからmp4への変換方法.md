---
id: 804
title: ffmpegでmovからmp4への変換方法
date: 2019-10-24T08:49:13+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2019/10/24/803-revision-v1/
permalink: /?p=804
---
### mov to mp4

```
ffmpeg -i medium.MOV -vcodec h264 -acodec mp2 medium.mp4
```

### mov to animation gif

```
$ ffmpeg -i medium.mov -r 24 medium.gif
```