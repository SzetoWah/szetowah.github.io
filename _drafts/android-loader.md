---
title: Android MVP模式
date: 2015-04-11
---

Loader 机制是在Android 3.0之后加入Android Framework，其目的在于方便开发人员在Activity和Fragment中异步地加载数据。

Loader有如下特征：
1. 提供异步加载数据机制
2. 对数据源变化进行监听，实时更新数据
3. 在Activity配置发生变化（如横竖屏切换）时，无需重复加载数据
4. 适用于任何Activity和Fragment
