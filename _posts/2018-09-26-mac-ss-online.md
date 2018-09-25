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

科学上网的最佳方式，是使用路由器上网，其次是使用客户端或命令行。部署代理服务的话，可以考虑 shadowsocks、v2ray

## Shadowsocks

官网：[shadowsocks](https://shadowsocks.org/)

自行购买 VPS，如 [搬瓦工](https://bandwagonhost.com/)、[vultr](https://www.vultr.com/)。VPS 超过 3 个的话，可以考虑使用 [shadowsocks-manager](https://github.com/shadowsocks/shadowsocks-manager)，以订阅方式提供代理服务。

服务搭建比较简单，可以参考 [shadowsocks-go](https://github.com/shadowsocks/shadowsocks-go)、[shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev)、[shadowsocks-manager](https://github.com/shadowsocks/shadowsocks-manager)

## v2ray

官网：[v2ray](https://www.v2ray.com/)

使用 v2ray，一定几率可以解救因使用 shadowsocks 而被识别封锁的 IP。

服务搭建也比较简单，可以参考 [v2ray](https://www.v2ray.com/)

## 路由器

路由器代理的方式因不同平台路由器而有所差异，此处，推荐使用 [梅林固件](http://koolshare.cn/)。

* -> [http://koolshare.cn/](http://koolshare.cn/)
* -> [固件下载](http://firmware.koolshare.cn/) 
* -> 选择相应路由器的型号，刷入固件 
* -> 安装 `科学上网` 插件，[Merlin_Shadowsocks](https://github.com/heweiye/Merlin_Shadowsocks)([koolshare 官网](https://github.com/koolshare/koolshare.github.io) 已下线 ss 插件)
* 配置，参考论坛即可

## 客户端



* [Proxifier](https://www.proxifier.com/)
* [SwitchyOmega](https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif)，[备用下载](https://github.com/FelisCatus/SwitchyOmega/releases)

