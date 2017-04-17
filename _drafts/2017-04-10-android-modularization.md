---
layout:     post
title:      "Android Modularization"
subtitle:   "模块化：modularization"
date:       2017-04-10
author:     "StuartHua"
header-img: "img/post-bg-2017.jpg"
catalog: true
tags:
    - Notes
    - Android
---

## 组件化 & 模块化

模块化早已不是新概念，最早应用于服务端，前端开发和移动端开发也发展了各自的开发方式。

<!-- more -->

* 模块化

模块化就是将一个程序按照其功能做拆分，分成相互独立的模块，以便于每个模块只包含与其功能相关的内容。模块我们相对熟悉,比如登录功能可以是一个模块,搜索功能可以是一个模块,汽车的发送机也可是一个模块.

* 组件化

组件化就是基于可重用的目的，将一个大的软件系统按照分离关注点的形式，拆分成多个独立的组件，已较少耦合。

* 模块化 vs 组件化

通俗来讲，模块化和组件化本质思想是一样的，都是“大化小”，两者的目的都是为了重用和解耦，只是叫法不一样。非要区分的话：  

1. 模块化强调的是拆分，无论是从业务角度还是从架构、技术角度，模块化首先意味着将代码、数据等内容按照其职责不同分离，使其变得更加容易维护、迭代，使开发人员可以分而治之
2. 组件化则着重于可重用性，不管是界面上反复使用的用户头像按钮，还是处理数据的流程中的某个部件，只要可以被反复使用，并且进行了高度封装，只能通过接口访问，就可以称其为“组件”。

* 为什么要进行组件化？

组件化开发的好处显而易见，最终结果是：每个组件都有自己独立的版本，可以独立的编译、测试、打包、部署。 最本质的好处是满足了产品的可交付状态！

## Android Modularization

随着业务的增长导致项目的复杂度增加，为了更好地适应团队开发，提高开发效率，十分有必要进行组件化。