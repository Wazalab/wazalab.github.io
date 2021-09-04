---
id: 170
title: '[React Native] Androidアプリのバージョンの指定方法'
date: 2017-04-29T10:14:21+09:00
author: Shohei
layout: revision
guid: http://www.wazalab.com/2017/04/29/160-revision-v1/
permalink: /?p=170
---
Androidは開発したことないのでわからないことが多い。　
`android/app/build.gradle`の`versionCode`と`versionName`を変更すればいい。

```
android {
    compileSdkVersion 23
    buildToolsVersion "23.0.1"

    defaultConfig {
        applicationId "com.wazalab.kagami"
        minSdkVersion 16
        targetSdkVersion 22
        versionCode 2  // ** this one
        versionName "1.0" // ** this one
        vectorDrawables.useSupportLibrary = true
        ndk {
            abiFilters "armeabi-v7a", "x86"
        }
    }
```

ちなみにこの変数は、

* versionName　- XcodeのVersionと同じ。ただじ数字でなくてもよい。
* versionCode - XcodeのBuildと同じ。ビルドごとに変更しないとpushできない。 


参考: [http://stackoverflow.com/questions/35924721/how-to-update-version-number-of-react-native-app](http://stackoverflow.com/questions/35924721/how-to-update-version-number-of-react-native-app)