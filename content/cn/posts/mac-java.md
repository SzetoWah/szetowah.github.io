---
title: "Mac 高效使用指南（一）：系统配置优化"
date: 2018-09-01
draft: false
categories: ['技术']
---

> 简单是有成本的。即便如此，面对重复繁琐的低效工作流程，我们还是应该 `Say No`。
> 本文旨在整理 Mac 高效使用技巧、提高程序员效率

## Mac 系统配置优化

### 触摸板优化

* 开启轻按点击功能

系统偏好设置 -> 触摸板 -> 光标与点按 -> 选中 `轻点来点按`

![WechatIMG4](/assets/img/media/WechatIMG4.png)

* 开启三指拖移

系统偏好设置 -> 辅助功能 -> 互动 `鼠标与触控板` -> 触控板选项 -> 勾选 `启用拖移`，选中 `三指拖移`

![WechatIMG5](/assets/img/media/WechatIMG5-1.png)

* 设置屏幕保护程序

初衷是保护离开屏幕时被窥探，可选择设置

系统偏好设置 -> 桌面与屏幕保护程序 -> 屏幕保护程序 -> 触发角 -> 活跃的屏幕角，选择一个角设置为 `将显示器置于睡眠状态`

![WechatIMG6](/assets/img/media/WechatIMG6.png)

### F1 - F12 标准按键

默认情况下，键盘最上面一排的 F1- F12 都有各自的作用，从图标上就能看出。比如 F1 和 F2 用来调节屏幕亮度，F5 和 F6 调节键盘灯亮度灯。程序员频繁使用 Debug，要使用 F1 - F12，便需要多按键盘左下角的 `fn 键`，所以，建议将 F1 - F12 改为标准按键

系统偏好设置 -> 键盘 -> 键盘，勾选 `将F1、F2等键用作标准功能键`

![WechatIMG9](/assets/img/media/WechatIMG9.png)

### 允许安装任意来源下载的应用

方便安装 `.dmg` 应用安装包

系统偏好设置 -> 安全性与隐私 -> 通用 -> 允许从一下位置下载的应用（需要点击左下方小锁进行设置）

![WechatIMG8](/assets/img/media/WechatIMG8.png)

### 开启全键盘控制

很多操作都会弹出系统的对话框，要求我们确认或者取消，如果没有开启完全键盘控制，我们只能按回车键确认，或者移动鼠标选择取消。如果开启了完全键盘控制，只要按下空格键，就相当于选中蓝色边框的按钮，也就是这里的取消键。按下 Tab 键可以在多个按钮之间切换。

系统偏好设置 -> 键盘 -> 快捷键 -> 全键盘控制，选择 `所有控制`

> 也可以使用 `Control + F7` 快捷设置

![1537864646793](/assets/img/media/1537864646793.jpg)

### 常用快捷键拓展

终端 Shell 中，经常用到 Emacs 系的快捷键，比起系统的一些按键更为方便快捷。

常见的几个行级别操作有：

* Control + A：移动到行首
* Control + E：移动到行尾
* Control + K：删除到行尾
* Control + U：删除到行头
* Control + N：移动到下一行
* Control + P：移动到上一行

> 前三个命令常用在终端中
> 后两个命令则在 Vim 系列中很常见，多用于上下切换列表中的选项

常见的几个字母级别操作有：

* Control + F：向右（Forward）移动一个字母，等价于 `方向键 →`
* Control + B：向左（Backward）移动一个字母，等价于 `方向键 ←`
* Control + D：向右删除一个字母，等价于 `→ + Delete` 这个快捷键也很常用
* Control + H：向左删除一个字母，等价于 `Delete`

Option 按单词操作：

> 按字母操作太过于细粒度，按行操作又太粗粒度， 不妨试试 Option，支持按单词操作。

* Option + ←：光标向左移动一个单词
* Option + →：光标向右移动一个单词
* Option + Delete：删除一个单词

### 拓展快速预览

对于一些文本文件，按下空格键就可以调用系统的预览程序，快速浏览文件内容。但如果想获得更好的阅读体验，或支持更多类型文件的快速浏览，就需要通过插件来完成。

更多详情可见：[quick-look-plugins](https://github.com/sindresorhus/quick-look-plugins)

```
brew cask install qlcolorcode qlstephen qlmarkdown quicklook-json qlimagesize webpquicklook qlvideo provisionql quicklookapk
```

