---
id: 810
title: Released iOS/iPadOS app, Wazari Browser, to make use of external keyboard
date: 2019-11-27T11:29:16+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2019/10/24/790-autosave-v1/
permalink: /?p=810
---
&lt;!--more          '
[video autoplay="off" loop="on" width="960" height="540" mp4="https://www.wazalab.com/wp-content/uploads/2019/10/medium.mp4"][/video]

<br>

<hr />

I just released iOS/iPadOS browser. This browser has the following functions.

<ul>
<li>Customizable shortcuts to operate browser. e.g. Change tabs without touching screen.</li>
<li>Panes to split views vertiacally or horizontally.</li>
<li>Hit-A-Hint - without touching, click links to move pages.</li>
<li>Customizable modifiers. e.g. swap capslock with ctrl key.</li>
<li>Customizable default search engine - DuckDuckGo or Google</li>
<li>Exclude web sites not to use keymapping. Some dynamic web site doesn't use Input type=text or textarea, which Wazari keymapping doesn't work. But you can exclude these website so you can still type on it.</li>
<li>Histories to go back easily</li>
<li>(Optional and Paying serivce) Integrated to Wazaterm so you can terminal.</li>
</ul>

Wazari browser is my first open source project and also a sub project of <a href="https://www.wazaterm.com">Wazaterm</a>.

Download from <a href="https://apps.apple.com/us/app/wazari-browser/id1475585924?mt=8">App store</a>

<hr />

I use react-native(iOS only) to build this to catch up recent React/React Native. I will post what technique that I used for this with the following posts.