---
id: 836
title: Released iOS/iPadOS app, Wazari Browser, to make use of external keyboard
date: 2019-11-26T08:29:04+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2019/11/26/790-revision-v1/
permalink: /?p=836
---
[video autoplay="off" loop="on" width="960" height="540" mp4="https://www.wazalab.com/wp-content/uploads/2019/10/medium.mp4"][/video]

<br>

---

I just released iOS/iPadOS browser. This browser has the following functions.

* Customizable shortcuts to operate browser. e.g. Change tabs without touching screen.
* Panes to split views vertiacally or horizontally.
* Hit-A-Hint - without touching, click links to move pages.
* Customizable modifiers. e.g. swap capslock with ctrl key.
* Customizable default search engine - DuckDuckGo or Google
* Exclude web sites not to use keymapping. Some dynamic web site doesn't use Input type=text or textarea, which Wazari keymapping doesn't work. But you can exclude these website so you can still type on it.
* Histories to go back easily
* (Optional and Paying serivce) Integrated to Wazaterm so you can terminal.

Wazari browser is my first open source project and also a sub project of [Wazaterm](https://www.wazaterm.com).

Download from [App store](https://apps.apple.com/us/app/wazari-browser/id1475585924?mt=8)

---

I use react-native(iOS only) to build this to catch up recent React/React Native. I will post what technique that I used for this with the following posts.