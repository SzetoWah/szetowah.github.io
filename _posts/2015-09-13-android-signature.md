---
layout:     post
title:      "关于 Android 签名"
subtitle:   "About Android Signature"
date:       2015-09-13
author:     "StuartHua"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Notes
    - Android
    - signature
---

## Signature

keytool, zipalign, apksigner, jarsigner 等命令的使用

<!-- more -->

## keytool

keytool 是 JDK 自带的工具, 是 Java 秘钥和证书的管理工具，可以用于管理对称加密和非对称加密，将秘钥和证书存储在keystore中。

### 使用 keytool 生成一个私钥

```
keytool -genkey -v -keystore my-release-key.jks
-keyalg RSA -keysize 2048 -validity 10000 -alias app
```

本示例会提示您输入密钥库和密钥的密码，并为您的密钥提供“Distinguished Name”字段。随后，它将生成一个名为 `my-release-key.jks` 的密钥库文件。此密钥库包含一个有效期为 10,000 天的密钥。

* -genkey 产生证书文件，等同于 genkeypair
* -keystore 指定密钥库的.keystore文件中
* -keyalg 指定密钥的算法，这里指定为RSA(非对称密钥算法)
* -validity 为证书有效天数，这里我们写的是10000天
* -alias 产生别名

提示: 可重复使用此条命令，在同一密钥库中创建多条密钥对

>区分 keystore 与 alias  
keystore（或jks）文件有一个密钥，内部可以添加多个alias，而alias也是有密钥的，不过很多keystore生成的时候，选择了默认与keystore密钥相同，在此，请注意区分。

### 查看 keystore 信息

* 查看keystore参数信息
    ```
    keytool -list -v -keystore my-release.jks
    ```

* 使用别名查看keystore特定条目，会提示键入 keystore 的密钥（注意，不是alias的密钥）

    ```
    keytool -list -v -keystore my-release.jks -alias app
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
* 修改别名 alias

    ```
    keytool -changealias -keystore keystore.jks -alias alias_name -destalias new_alias_name
    ```
* 修改 alias 密码
    ```
    keytool -keypasswd -keystore keystore.jks -alias alias_name
    ```

## zipalign

zipalign 是对zip包对齐的工具,使APK包内未压缩的数据有序排列对齐,从而减少APP运行时内存消耗。

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

>zipalign可以在V1签名后执行  
但zipalign不能在V2签名后执行,只能在V2签名之前执行！！！

## 签署 APK

jarsigner是JDK提供的针对jar包签名的通用工具,
位于JDK/bin/jarsigner.exe

apksigner是Google官方提供的针对Android apk签名及验证的专用工具,
位于Android SDK/build-tools/SDK版本/apksigner.bat

不管是apk包,还是jar包,本质都是zip格式的压缩包,所以它们的签名过程都差不多(仅限V1签名), 这两个工具都可以对Android apk包进行签名.

### V1和V2签名的区别

在Android Studio中点击菜单 Build->Generate signed apk... 打包签名过程中, 可以看到两种签名选项 V1(Jar Signature)  V2(Full APK Signature)

从Android 7.0开始, 谷歌增加新签名方案 V2 Scheme (APK Signature);
但Android 7.0以下版本, 只能用旧签名方案 V1 scheme (JAR signing)

#### V1签名:
来自JDK(jarsigner), 对zip压缩包的每个文件进行验证, 签名后还能对压缩包修改(移动/重新压缩文件), 对V1签名的apk/jar解压,在META-INF存放签名文件(MANIFEST.MF, CERT.SF, CERT.RSA), 其中MANIFEST.MF文件保存所有文件的SHA1指纹(除了META-INF文件), 由此可知: V1签名是对压缩包中单个文件签名验证

#### V2签名:
来自Google(apksigner), 对zip压缩包的整个文件验证, 签名后不能修改压缩包(包括zipalign), 对V2签名的apk解压,没有发现签名文件,重新压缩后V2签名就失效, 由此可知: V2签名是对整个APK签名验证

V2签名优点:

* 签名更安全(不能修改压缩包)
* 签名验证时间更短(不需要解压验证),因而安装速度加快

注意: apksigner工具默认同时使用V1和V2签名,以兼容Android 7.0以下版本

### 使用

* 方式一、apksigner (apksigner, 默认同时使用V1和V2签名)

```
apksigner sign --ks my-release-key.jks my-app.apk
```

检测是否签名

```
apksigner verify my-app.apk
```

* 方式二、jarsigner (jarsigner, 只支持V1签名)

```
jarsigner -keystore 密钥库名 xxx.apk 密钥别名

# example
jarsigner -verbose -keystore my-release-key.jks -signedjar signed.apk unsigned.apk [alias]
```

参数:

* -digestalg  摘要算法
* -sigalg     签名算法

从JDK7开始, jarsigner默认算法是SHA256, 但Android 4.2以下不支持该算法, 所以需要修改算法, 添加参数 -digestalg SHA1 -sigalg SHA1withRSA

```
jarsigner -keystore 密钥库名 -digestalg SHA1 -sigalg SHA1withRSA xxx.apk 密钥别名
```

例如 1：

* 用JDK7及以上jarsigner签名,不支持Android 4.2 以下
    ```
    jarsigner -keystore debug.keystore MyApp.apk androiddebugkey
    ```
* 用JDK7及以上jarsigner签名,兼容Android 4.2 以下            
    ```
    jarsigner -keystore debug.keystore -digestalg SHA1 -sigalg SHA1withRSA MyApp.apk androiddebugkey
    ```

例如 2：

有的市场（如360）会提示使用了SHA256签名算法，Android4.2之前的版本，无法识别SHA256签名算法，从而导致安装失败，解决方法如下：

```
jarsigner -digestalg SHA1 -sigalg SHA1withRSA -verbose -keystore my-release-key.jks -signedjar signed.apk unsigned.apk [alias]
```

检测是否签名

```
jarsigner  -verbose -certs -verify my-app.apk
# or
keytool -printcert -jarfile my-app.apk
```

## 参考

* [官方说明](https://developer.android.com/studio/publish/app-signing.html?hl=zh-cn#signing-manually)
* [Android-APK签名工具-jarsigner和apksigner](https://blog.csdn.net/qq_32115439/article/details/55520012)


