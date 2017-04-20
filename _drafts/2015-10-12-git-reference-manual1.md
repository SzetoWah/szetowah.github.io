---
layout:     post
title:      "Git 参考手册（一）"
subtitle:   "关于git的那点事儿"
date:       2015-10-12
author:     "StuartHua"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Notes
    - git
---

## 获取与创建项目

得先有一个 Git 仓库，才能用它进行操作。仓库是 Git 存放要保存的快照的数据的地方。

### git init

将一个项目初始化为git目录，操作是本地化的

### git clone

复制一个git仓库，

```
git clone <仓库地址> <本地目录名>

# for example
# <本地目录名> 可不填写
git clone git@github.com:google/googletv-android-samples.git
```

## 基本的快照

Git 的工作就是创建和保存你的项目的快照及与之后的快照进行对比。





