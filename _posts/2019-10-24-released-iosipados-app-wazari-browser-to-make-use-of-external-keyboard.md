---
id: 790
title: Released iOS/iPadOS app, Wazari Browser, to make use of external keyboard
date: 2019-10-24T11:10:12+09:00
author: Shohei
layout: post
guid: https://www.wazalab.com/?p=790
permalink: /2019/10/24/released-iosipados-app-wazari-browser-to-make-use-of-external-keyboard/
siteorigin_page_settings:
  - 'a:6:{s:6:"layout";s:7:"default";s:10:"page_title";b:1;s:15:"masthead_margin";b:1;s:13:"footer_margin";b:1;s:16:"display_masthead";b:1;s:22:"display_footer_widgets";b:1;}'
enclosure:
  - |
    https://www.wazalab.com/wp-content/uploads/2019/10/medium.mp4
    424464
    video/mp4
    
categories:
  - App
  - IT
  - Service
  - Side Projects
tags:
  - Browser
  - ios
  - iPadOS
  - OSS
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