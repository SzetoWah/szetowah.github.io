---
layout:     post
title:      "Android 签名"
subtitle:   "Android Signature"
date:       2015-09-13
author:     "StuartHua"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Notes
    - Android
    - signature
---

## Android Signature

keytool、zipalign、apksigner、jarsigner 等命令的使用

<!-- more -->

## keytool

keytool是一个秘钥和证书的管理工具，可以用于管理对称加密和非对称加密，将秘钥和证书存储在keystore中。

### 使用 keytool 生成一个私钥

```
keytool -genkey -v -keystore my-release-key.jks
-keyalg RSA -keysize 2048 -validity 10000 -alias app
```

本示例会提示您输入密钥库和密钥的密码，并为您的密钥提供“Distinguished Name”字段。随后，它将生成一个名为 my-release-key.jks 的密钥库文件。此密钥库包含一个有效期为 10,000 天的密钥。

* -genkey 产生证书文件
* -keystore 指定密钥库的.keystore文件中
* -keyalg 指定密钥的算法，这里指定为RSA(非对称密钥算法)
* -validity 为证书有效天数，这里我们写的是10000天
* -alias 产生别名

>区分密钥  
jks（或keystore）文件有一个密钥，内部可以添加多个alias，而alias也是有密钥的，不过很多keystore生成的时候，选择了默认与keystore密钥相同，在此，请注意区分。

### 查看 keystore 信息

* 列出keystore存在的所有证书

```
keytool -list -keystore my-release.jks
```

* 使用别名查看keystore特定条目，会提示键入 keystore 的密钥（注意，不是alias的密钥）

```
keytool -list -keystore my-release.jks -alias app
```

可获得 MD5、SHA1、SHA256 等信息

### 其他 Keytool 命令

* 删除keystore里面指定证书

```
keytool -delete -alias mydomain -keystore keystore.jks
```

* 更改keysore密码

```
keytool -storepasswd -new new_storepass -keystore keystore.jks
```

## zipalign

zipalign 使用4字节的边界对齐方式来影射内存，通过空间换时间的方式提高执行效率。按着有利于系统处理的排列方式，对我们apk中的资源文件进行排列，提高资源的查找速度，从而去提高应用的运行效率。

* -c  ：检查.apk文件是否zipalign优化过
* -f  ：覆盖已经存在的文件
* -p ：页面对其存储对象文件
* -v ：输出优化后的详细信息 xx.apk
* -z ：将使用Zopfli

```
# 验证是否zipalign
zipalign -v -c 4 myapp.apk

# 优化 apk
zipalign -v 4 source.apk destination.apk
```

## 签署 APK

* 方式一、apksigner

```
apksigner sign --ks my-release-key.jks my-app.apk
```

检测是否签名

```
apksigner verify my-app.apk
```

* 方式二、jarsigner

```
jarsigner -verbose -keystore my-release-key.jks -signedjar signed.apk unsigned.apk [alias]
```

有的市场（如360）会提示使用了SHA256签名算法，但Android4.2之前的版本，无法识别SHA256签名算法，从而导致安装失败，解决方法如下：

```
jarsigner -digestalg SHA1 -sigalg SHA1withRSA -verbose -keystore my-release-key.jks -signedjar signed.apk unsigned.apk [alias]
```

检测是否签名

```
jarsigner  -verbose -certs -verify my-app.apk
```

## 参考

* [官方说明](https://developer.android.com/studio/publish/app-signing.html?hl=zh-cn#signing-manually)


