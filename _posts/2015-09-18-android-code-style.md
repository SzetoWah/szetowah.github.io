---
layout:     post
title:      "Android Studio 代码格式设置"
subtitle:   "Anroid Code Style"
date:       2015-09-18
author:     "StuartHua"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Notes
    - Android
    - code style
---

## Anroid Code Style

通常，Code Style 可参考 suare 或 google 的规范

### 1. Square Style

[java-code-styles](https://github.com/square/java-code-styles)

安装

```
cd java-code-styles
./install.sh
```

打开 Android Studio， Android Studio -> Preferences

![code-style-for-mac-5](http://7o50rs.com1.z0.glb.clouddn.com/2017-04-18-code-style-for-mac-5.png)

找到 Editor -> Code Style -> Java,在 Scheme 下拉框中选择 SquareAndroid，点击 ok

![code-style-for-mac-6](http://7o50rs.com1.z0.glb.clouddn.com/2017-04-18-code-style-for-mac-6.png)

快捷键 `Command + Alt + L`， 即可对之前的代码进行格式化

>Square 规范中，tab 默认2个字符，可以逐一点击，进行更改，Tab size : 4

### 2. Google Style [styleguide](https://github.com/google/styleguide)

```
styleguide/intellij-java-google-style.xml
```

打开 Android Studio，Android Studio -> Preferences

![code-google-style](http://7o50rs.com1.z0.glb.clouddn.com/2017-04-18-code-google-style.png)

找到 Editor -> Code Style -> Java，点击Manage -> Import，选中 `styleguide/intellij-java-google-style.xml`，即可







