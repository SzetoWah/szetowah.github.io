---
layout:     post
title:      "Android Keytool"
subtitle:   "Android 签名证书相关"
date:       2017-01-16
author:     "StuartHua"
header-img: "img/post-bg-2017.jpg"
catalog: true
tags:
    - Notes
---

## Keytool

keytool是一个秘钥和证书的管理工具，可以用于管理对称加密和非对称加密，将秘钥和证书存储在keystore中。

[官方说明](https://developer.android.com/studio/publish/app-signing.html?hl=zh-cn#signing-manually)

## 使用 keytool 生成一个私钥

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

## 查看 keystore 信息

* 列出keystore存在的所有证书

```
keytool -list -keystore my-release.jks
```

* 使用别名查看keystore特定条目，会提示键入 keystore 的密钥（注意，不是alias的密钥）

```
keytool -list -keystore my-release.jks -alias app

# 输出，如
Alias name: androiddebugkey
Creation date: Jun 21, 2015
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 7b9d299c
Valid from: Sun Jun 21 12:05:22 CST 2015 until: Tue Jun 13 12:05:22 CST 2045
Certificate fingerprints:
	 MD5:  E6:DA:79:4D:5D:A1:AF:B6:3B:7D:A3:B5:AD:37:7A:A1
	 SHA1: 9F:AC:1A:2F:5C:72:AD:29:DB:EB:F7:1D:36:F8:B7:0F:02:C0:C0:86
	 SHA256: 81:58:1C:F6:2A:4F:38:43:5C:16:58:BE:A7:45:58:7C:B8:AE:42:FD:4B:B4:3B:EC:B9:4E:E3:82:8B:FA:66:F2
	 Signature algorithm name: SHA256withRSA
	 Version: 3

Extensions:

#1: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: 7F E1 F3 7A 84 58 F6 D6   5C 73 66 94 B7 E8 58 F3  ...z.X..\sf...X.
0010: 49 06 7E 6D                                        I..m
]
]
```

可获得 MD5、SHA1、SHA256 等信息

## 其他Keytool命令

* 删除keystore里面指定证书

```
keytool -delete -alias mydomain -keystore keystore.jks
```

* 更改keysore密码

```
keytool -storepasswd -new new_storepass -keystore keystore.jks
```

## 签署 APK

```
apksigner sign --ks my-release-key.jks my-app.apk
```

