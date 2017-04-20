---
layout:     post
title:      "AndroidMainfest allowBackup supportsRtl"
subtitle:   "AndroidMainfest"
date:       2016-08-13
author:     "StuartHua"
header-img: "img/post-bg-2016.jpg"
catalog: true
tags:
    - Notes
    - Android
    - mainfest
---

Android API Level 8 及其以上 Android 系统提供了为应用程序数据的备份和恢复功能，此功能的开关决定于该应用程序中 AndroidManifest.xml 文件中的 allowBackup 属性值，其属性值默认是 True。当 allowBackup 标志为 true 时，用户即可通过 adb backup 和 adb restore 来进行对应用数据的备份和恢复，这可能会带来一定的安全风险。

## allowBackup

https://segmentfault.com/a/1190000002590577

## supportRtl

http://blog.csdn.net/aa464971/article/details/51372204

