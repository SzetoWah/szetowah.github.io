---
layout:     post
title:      "Mac 高效使用指南（五）：科学上网"
subtitle:   "Mac  Effective Use Guide 5 - Proxy Online"
date:       2018-09-04
author:     "StuartHua"
header-img: "assets/img/post-bg-2018.jpg"
catalog: true
tags:
    - mac
---

[TOC]

科学上网的最佳方式，是使用路由器上网，其次是使用客户端或命令行。部署代理服务的话，可以考虑 shadowsocks、shadowsocksrr、v2ray

## 服务

### Shadowsocks & Shadowsocksrr

官网：[shadowsocks](https://shadowsocks.org/)，[shadowsocksrr](https://github.com/shadowsocksrr)

自行购买 VPS，如 [搬瓦工](https://bandwagonhost.com/)、[vultr](https://www.vultr.com/)。VPS 超过 3 个的话，可以考虑使用 [shadowsocks-manager](https://github.com/shadowsocks/shadowsocks-manager)、[ss-panel](https://github.com/search?q=ss+panel)，以订阅方式提供 ss 服务。

* `ss-panel` 使用 php、mysql 搭建，低内存 VPS 性能比较弱，一般需要 2G 内存；
* `shadowsocks-manager` 使用 node、SQLite（mysql），对性能要求相对较小。

个人使用的话，更推荐使用 `shadowsocks-manager`，但只能使用 shadowsocks，不能添加 ssr 的混淆。可自行根据 VPS 性能取舍。

> 注意：不管 `ss-panel`、`shadowsocks-manager` 都是用来管理 shadowsocks 或 shadowsocksrr 服务的系统，都需要搭建 shadowsocks/shadowsocksrr 服务。而搭建 shadowsocks/shadowsocksrr 服务，又分为不同语言版本的实现，如 [shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev)、[shadowsocks-go](https://github.com/shadowsocks/shadowsocks-go) ([shadowsocksr-libev](https://github.com/shadowsocksrr/shadowsocksr-libev)、[shadowsocksr](https://github.com/shadowsocksrr/shadowsocksr))，即 python、go、C 版本，不管语言版本为何，都会实现代理的功能，包含 ss-manager、ss-local 服务。

此处，以 [shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev)、[shadowsocks-manager](https://github.com/shadowsocks/shadowsocks-manager) 为例，搭建 ss 服务。

#### shadowsocks-libev

[Github:shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev)

搭建指南：[翻墙教程 - 基础篇](https://medium.com/@stuarthua/%E7%BF%BB%E5%A2%99%E6%95%99%E7%A8%8B-%E5%9F%BA%E7%A1%80%E7%AF%87-2a886ca4c5b4)

> 有多台 VPS，选择使用 ssm 管理 ss 服务，无需手动开启 shadowsocks-libev 服务，交由 ssm 管理即可。

拓展阅读：优化 [翻墙教程 - 中级篇](https://medium.com/@stuarthua/%E7%BF%BB%E5%A2%99%E6%95%99%E7%A8%8B-%E4%B8%AD%E7%BA%A7%E7%AF%87-e79cf006656b)

#### 可选：shadowsocks-manager

[Github:shadowsocks-manager](https://github.com/shadowsocks/shadowsocks-manager)

搭建指南：[翻墙教程 - SS-Manager](https://medium.com/@stuarthua/%E7%BF%BB%E5%A2%99-ss-manager-bcad7be0c2dc)

> 有多台 VPS，厌烦了每次扫描二维码或输入配置信息，希望提供分享给其他终端，可以考虑使用 ssm 管理 ss 服务。

### v2ray（可选）

官网：[v2ray](https://www.v2ray.com/)

除了 shadowsocks，还有其他的代理方式，如 v2ray。而且使用 v2ray，一定几率可以解救因使用 shadowsocks 而被标识，遭到封锁的 IP。相应的，服务端、客户端需要单独的软件。也有一些软件可以同时兼容 ss、v2ray，下文会有介绍。

搭建指南：[翻墙教程 - 高级篇](https://medium.com/@stuarthua/%E7%BF%BB%E5%A2%99%E6%95%99%E7%A8%8B-%E9%AB%98%E7%BA%A7%E7%AF%87-516b6253f101)

## 平台

### 路由器

路由器代理的方式因不同平台路由器而有所差异，推荐使用 [梅林固件](http://koolshare.cn/)。

* -> [http://koolshare.cn/](http://koolshare.cn/)
* -> [固件下载](http://firmware.koolshare.cn/) 
* -> 选择相应路由器的型号，刷入固件 
* -> 安装 `科学上网` 插件，[Merlin_Shadowsocks](https://github.com/heweiye/Merlin_Shadowsocks)([koolshare 官网](https://github.com/koolshare/koolshare.github.io) 已下线 ss 插件)
* -> 配置，参考论坛即可

参考地址：[零基础AC68U刷梅林](http://koolshare.cn/thread-48190-1-1.html)

配置：路由器本身的设置比较繁琐，如节点负载、加速器等，都需要一定时间了解，而搭载插件中心后，一些特定插件的设置也有一定门槛。因为网络环境差异，此处不做详细说明。

### 客户端

#### Mac

* [ShadowsocksX-NG](https://github.com/shadowsocks/ShadowsocksX-NG) 不支持订阅
* [ShadowsocksX-NG-R](https://github.com/qinyuhang/ShadowsocksX-NG-R/releases) 支持订阅

ShadowsocksX-NG 使用指南：[Mac ShadowsocksX-NG 使用教程](https://medium.com/@stuarthua/mac-shadowsocksx-ng-%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B-eeeeb196679)

ShadowsocksX-NG-R 使用指南：[Mac ShadowsocksX-NG-R 使用教程](https://medium.com/@stuarthua/mac-shadowsocksx-ng-r-%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B-aa68dceab9ba)

关于二者的区别，可以查看：[关于ShadowsocksX-NG各个版本的疑问](https://github.com/shadowsocks/ShadowsocksX-NG/issues/143)

#### Windows

[Windows SS SSR 使用指南](https://medium.com/@stuarthua/windows-ss-ssr-%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97-27df9cbd8569)

#### iOS

[iOS Shadowrocket 使用教程](https://medium.com/@stuarthua/ios-shadowrocket-%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B-e0384f0d4d3e)

#### Android

[Android SS SSR 使用指南](https://medium.com/@stuarthua/android-ss-ssr-%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97-442be26844bc)