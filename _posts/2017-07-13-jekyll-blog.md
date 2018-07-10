---
layout:     post
title:      "Jekyll 独立博客搭建"
subtitle:   "Jekyll Blog"
date:       2017-07-13
author:     "StuartHua"
header-img: "img/post-bg-2017.jpg"
catalog: true
tags:
    - Notes
    - jekyll
---

## Jekyll

参考 [Jekyll](https://jekyllcn.com/)

Jekyll 不再需要数据库，不需要开发评论功能，不需要不断的更新版本, —— 只关心你的博客内容。

### 安装

1. 安装 ruby 环境 (参考[ruby 环境搭建](https://blog.stuarthua.com/2017/07/12/centos-software/#ruby))

2. 安装 jekyll
    ```
    gem install jekyll
    ```

### 基本用法

```
$ jekyll build
# => 当前文件夹中的内容将会生成到 ./_site 文件夹中。

$ jekyll build --destination <destination>
# => 当前文件夹中的内容将会生成到目标文件夹<destination>中。

$ jekyll build --source <source> --destination <destination>
# => 指定源文件夹<source>中的内容将会生成到目标文件夹<destination>中。

$ jekyll build --watch
# => 当前文件夹中的内容将会生成到 ./_site 文件夹中，
#    查看改变，并且自动再生成。
```

## 预览

Jekyll 同时也集成了一个开发用的服务器，可以让你使用浏览器在本地进行预览。

```
$ jekyll serve
# => 一个开发服务器将会运行在 http://localhost:4000/
# Auto-regeneration（自动再生成文件）: 开启。使用 `--no-watch` 来关闭。

$ jekyll serve --detach
# => 功能和`jekyll serve`命令相同，但是会脱离终端在后台运行。
#    如果你想关闭服务器，可以使用`kill -9 1234`命令，"1234" 是进程号（PID）。
#    如果你找不到进程号，那么就用`ps aux | grep jekyll`命令来查看，然后关闭服务器。[更多](http://unixhelp.ed.ac.uk/shell/jobz5.html).
```

## 主题

参考 [http://huangxuan.me](https://github.com/Huxpro/huxpro.github.io)

>拓展功能 jekyll-search  
[jekyll-search](https://github.com/androiddevelop/jekyll-search)