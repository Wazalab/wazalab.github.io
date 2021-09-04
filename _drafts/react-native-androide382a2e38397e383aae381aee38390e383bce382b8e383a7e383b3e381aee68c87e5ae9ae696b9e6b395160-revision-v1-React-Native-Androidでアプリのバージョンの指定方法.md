---
id: 161
title: '[React Native] Androidでアプリのバージョンの指定方法'
date: 2017-04-29T09:57:03+09:00
author: Shohei
layout: revision
guid: http://www.wazalab.com/2017/04/29/160-revision-v1/
permalink: /?p=161
---

Androidは開発したことないのでわからないことが多い。　
`android/app/build.gradle`の`versionCode`と`versionName`を変更すればう良い。

```
android {
    compileSdkVersion 23
    buildToolsVersion "23.0.1"

    defaultConfig {
        applicationId "com.wazalab.kagami"
        minSdkVersion 16
        targetSdkVersion 22
        versionCode 2
        versionName "1.0"
        vectorDrawables.useSupportLibrary = true
        ndk {
            abiFilters "armeabi-v7a", "x86"
        }
    }
```


参考: http://stackoverflow.com/questions/35924721/how-to-update-version-number-of-react-native-app