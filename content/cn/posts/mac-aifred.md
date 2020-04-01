---
title: "Mac 高效使用指南（四）：Alfred 使用"
date: 2018-09-04
draft: false
categories: ['技术']
---

Alfred 一款提供基本搜索、快速启动和使用自定义插件的工作流功能。除了软件自带的功能外，它还允许用户自己开发 workflow 并分享出来。

## General 界面

* 开机自启动
* 快速启动 `Option + Space` 
* Where are you：你在哪个国家。Alfred内置了常用网站搜索功能，在搜索时打开对应国家的网站。

![1537895027027](/assets/img/media/1537895027027.jpg)

## Features 特性界面

免费版能够体验到的服务界面，Alfred 里所有的搜索功能都在这里设置。

#### Default Results

Alfred 建议用户使用 `Find + Space +文件名` 来查询文件或文件夹，活着使用 `Open + Space +文件名`

#### Web Search

点击右下角的 `Add Custom Search`

根据示意图在弹出界面中输入对应信息，其中网站图标可忽略，然后保存即可。
(/assets/img/media/
![1537889221332](/assets/img/media/1537889221332.jpg)

常用网站：

* 百度：https://www.baidu.com/s?ie=utf-8&f=8&wd={query}
* 简书：https://www.jianshu.com/search?utf8=%E2%9C%93&q={query}
* 淘宝：https://s.taobao.com/search?oe=utf-8&f=8&q={query}
* 京东：https://search.jd.com/Search?keyword={query}&enc=utf-8&wq={query}
* 微信文章：https://weixin.sogou.com/weixin?type=2&query={query}
* stackoverflow：https://www.stackoverflow.com/search?q={query}
* github：https://github.com/search?utf8=%E2%9C%93&q={query}
* Android API Search：https://developer.android.com/reference/classes.html#q={query}

#### Dictionary

设置词典的关键词 di：

![1537889425327](/assets/img/media/1537889425327.jpg)

#### Clipboard

开启Alfred的剪切板功能

![1537889380835](/assets/img/media/1537889380835.jpg)

查看 Alfred 剪切板历史记录：

双击 Option

![1537889775902](/assets/img/media/1537889775902.jpg)

#### System

「清空回收站」的关键字设置为 em，勾选 `Confirm`

![1537890029060](/assets/img/media/1537890029060.jpg)

![1537890045764](/assets/img/media/1537890045764.jpg)

#### Terminal/Shell

更改默认终端，配置为 iTerm2。

在 Application 中选择 `custom`，配置如下内容：

```
on write_to_file(this_data, target_file, append_data)
    try
        set the target_file to the target_file as string
        set the open_target_file to open for access file target_file with write permission
        if append_data is false then set eof of the open_target_file to 0
        write this_data to the open_target_file starting at eof
        close access the open_target_file
        return true
    on error
        try
            close access file target_file
        end try
        return false
    end try
end write_to_file


on alfred_script(q)
    
    -- Write the command to run to a file. This is done because Applescript quoting is impossible to get right, esp. for backslashes.
    set tmp_dir to path to temporary items as string from user domain
    set applescript_alfred_file to tmp_dir & "alfredscript"
    set alfred_file to POSIX path of applescript_alfred_file
    write_to_file(q & return, applescript_alfred_file, false)
    
    -- Create this file, which prevents iTerm2 from restoring a saved window arrangement.
    do shell script "touch ~/Library/Application' Support/iTerm/quiet'"
    
    -- Test cases:
    -- 1. iTerm2 running, has windows open. Should open a new window for Alfred command.
    -- 2. iTerm2 running, no windows open. Should open a new window for Alfred command.
    -- 3. iTerm2 not running, set to restores arrangement. Should not restore arrangement but open a new window for the Afred command.
    -- 4. iTerm2 not running. No windows to restore. Should open a single window for the Alfred command.
    -- 5. iTerm2 not running. Has windows to restore. Restores windows and then opens a new window for the Alfred command.
    
    -- Compose a script. This is necessary because compiling in a 'tell application' command causes the app to be launched, which would happen prior to the creation of the quiet file.
    set theScript to "tell application \"iTerm2.app\"
    if (exists current window) then
        tell current window to create tab with default profile
        tell current session of current window
            write contents of file \"" & alfred_file & "\"
        end tell
    else
        create window with default profile
        tell current session of current window
            write contents of file \"" & alfred_file & "\"
        end tell
    end if
    activate
end tell"
    
    -- Invoke the script.
    run script theScript
    
    -- Clean up
--    do shell script "rm -f ~/Library/Application' Support/iTerm/quiet' /tmp/alfredscript"
end alfred_script
```

>iTerm2 版本不同的话，此项配置有可能不生效，此处，iTerm2 版本 > 3.0

## Appearance

Appearance 面板除了设置输入窗口的外观外，还有一些外观相关的设置，在这里可以设置默认展示行数等

![appearance](/assets/img/media/appearance.png)


## Advanced

配置同步，在 Advanced 选项右下角，选择同步目录：

![-w486](/assets/img/media/15378898397583.jpg)

其他设置：

![alfred-advanced](/assets/img/media/alfred-advanced.png)

## Workflow

一些常用的 Alfred workflow：

* [codeVar](https://github.com/xudaolong/CodeVar)

    >生成变量名，支持大小驼峰、常量、下划线

![1537891723497](/assets/img/media/1537891723497.jpg)

* [Dash](https://github.com/Kapeli/Dash-Alfred-Workflow)

    >安装 Dash -> Preferences > Integration > Alfred

![1537891846281](/assets/img/media/1537891846281.jpg)

* [有道翻译](https://github.com/liszd/whyliam.workflows.youdao)

![1537892346922](/assets/img/media/1537892346922.jpg)

* [AlfredHiddenFilesToggle](https://github.com/logic1988/AlfredHiddenFilesToggle)

    > macOS 显示隐藏文件

* [alfred_kuaidi](https://github.com/roylez/alfred_kuaidi)

    > 查询快递

![kuaidi](/assets/img/media/kuaidi.png)

### workflow 插件搜索

* [http://www.packal.org/](http://www.packal.org/)
* [http://www.alfredworkflow.com/](http://www.alfredworkflow.com/)
* [github](https://github.com/search?q=alfred-workflows)
