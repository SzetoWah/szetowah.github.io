---
title: Android MVP模式
date: 2015-04-11
---
MCP, 全称 Model－View－Presenter，其中 Presenter 解耦了 Model 与 View，使得每个模块的职责更加单一，Model 负责获取数据，View 只关心视图的绘制，Presenter 关联 Model 和 View 处理业务逻辑。<!-- more -->

## M (Model)
模型：表示数据模型和业务逻辑。模型并不总是DataSet，DataTable之类的，它代表着一类组件或类，这些组件和类可以向外部提供数据，同时也能从外部获取数据并将这些数据存储在某个地方。

Model层主要责任：

* 从网络、数据库、文件、传感器、第三方等数据源写数据
* 对外部的数据类型进行解析转换为APP内部数据，并交由上层处理
* 对数据的临时存储、管理，协调上层数据请求

## V (View)
视图：将数据呈现给用户。

View层主要责任：

* 提供UI交互
* 在Presenter的控制下修改UI
* 将业务事件交给Presenter处理

注意，View层不存储数据，不于Model层交互。Android中，View层一般是Actviity、Fragment、View、ViewGroup等

## P (Presenter)
层现器：作为View与Model交互的中间纽带，处理与用户交互的逻辑。Presenter包含了根据用户在视图中的行为去更新模型的逻辑，视图仅仅只是将用户的行为告知Presenter，而Presenter负责从视图中取得数据然后发送给模型

MVP 模式作为用来分离UI层与业务层的一种开发模式。
