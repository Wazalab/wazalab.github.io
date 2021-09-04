---
id: 164
title: '[React Native] Androidアプリのバージョンの指定方法'
date: 2017-04-29T10:10:18+09:00
author: Shohei
layout: revision
guid: http://www.wazalab.com/2017/04/29/160-autosave-v1/
permalink: /?p=164
---
Androidは開発したことないのでわからないことが多い。　
<code>android/app/build.gradle</code>の<code>versionCode</code>と<code>versionName</code>を変更すればいい。

<pre><code>android {
    compileSdkVersion 23
    buildToolsVersion "23.0.1"

    defaultConfig {
        applicationId "com.wazalab.kagami"
        minSdkVersion 16
        targetSdkVersion 22
        versionCode 2  // ** this one
        versionName "1.0" // **thi
        vectorDrawables.useSupportLibrary = true
        ndk {
            abiFilters "armeabi-v7a", "x86"
        }
    }
</code></pre>

ちなみに
* versionName　- XcodeのVersionと同じ。ただじ数字でなくてもよい。
* versionCode - XcodeのBuildと同じ

参考: http://stackoverflow.com/questions/35924721/how-to-update-version-number-of-react-native-app