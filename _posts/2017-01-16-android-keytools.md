---
layout:     post
title:      "Android Keytool"
date:       2017-01-16
author:     "Stuart"
header-img: "img/post-bg-2017.jpg"
tags:
    - Notes
---

Android Keytool 生成签名证书相关

## Generate

```
keytool -genkey -v -alias myapp -keyalg RSA -keysize 1024 -keypass 123456 -validity 10000 -keystore my-release-key.jks -storepass 123456
```

## List 

```
keytool -list  -v -keystore my-release-key.jks -storepass 123456
```

## 使用别名查看keystore特定条目

```
keytool -list -v -keystore my-release-key.jks -storepass 123456 -alias myapp -storepass 123456
```


