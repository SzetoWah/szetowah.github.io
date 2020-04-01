---
title: "Mac 高效使用指南（二）：打造高效终端 zsh、oh-my-zsh、iterm2、tumx"
date: 2018-09-02
draft: false
categories: ['技术']
---

## zsh

官网：[zsh](http://www.zsh.org/)

Mac 系统自带了 zsh，但一般不是最新版，如果需要最新版可通过 Homebrew 来安装。

```
brew info zsh
brew install zsh
```

* 使用 `/bin/zsh --version` 查看系统 zsh 版本
* Homebrew 安装 zsh 后，使用 `zsh --version` 查看 zsh 版本

修改默认 Shell：

1. 在 `/etc/shells` 文件中加入如下一行
    ```
    $ sudo vi /etc/shells
    
    ## 增加
    /usr/local/bin/zsh
    ```
2. 然后运行命令：
    ```
    chsh -s /usr/local/bin/zsh
    ```
3. 重启当前 Tab 验证是否生效，`echo $SHELL`

> 小提示：
> 还原默认 Shell ：`chsh -s /bin/bash`

## oh-my-zsh

官网：[oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)

oh-my-zsh 用来管理 zsh 配置的，节省用户时间。

Curl 安装：

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

Wget 安装：

```
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

### 常用插件

官方插件：[Plugins-Overview](https://github.com/robbyrussell/oh-my-zsh/wiki/Plugins-Overview)

#### 插件安装方法

打开文件 `~/.zshrc`，配置：

```
# Add wisely, as too many plugins slow down shell startup.
plugins=(
  git autojump osx sublime extract zsh-syntax-highlighting zsh-autosuggestions zsh-completions
)
```

修改完后，如果需要在当前shell中生效，需要执行：

```
source ~/.zshrc
```

> 注意：
> 1. 插件安装太多，会影响 Shell 启动速度
> 2. 有些插件，oh-my-zsh 中自带，无需重复安装，直接启用即可；但也有一些插件需要安装一些依赖

#### git 插件

> 默认开启

oh-my-zsh 默认开启的插件，提供了大量 git 的alias。

[详情](https://github.com/robbyrussell/oh-my-zsh/wiki/Plugin:git)

#### extract 插件

> 直接开启即可，无需额外安装

功能强大的解压插件，所有类型的文件解压一个命令x全搞定，再也不需要去记tar后面到底是哪几个参数

#### sublime 插件

> 直接开启即可，无需额外安装

该插件可以使用命令行打开 sublime。

```
st          # 直接打开sublime
st fileXX   # 用sublime打开文件 file
st dirXX    # 用sublime打开目录 dir
stt         # 在sublime打开当前目录，相当于 st .
```

#### osx 插件

> 直接开启即可，无需额外安装

* tab - 在一个新标签打开当前目录
* cdf - cd 到当前 Finder 目录
* quick-look - 快速浏览特殊的文件
* man-preview - 在 Preview 应用打开特定的 man page
* trash - 将特定的文件移到垃圾桶

使用：

1. 各种补全：除了支持命令补全和文件补全之外，还支持命令参数补全，插件内容补全，只需要按 tab 键
2. 目录浏览和跳转：输入 d，就显示在会话里访问的目录列表，输入列表前的序号，即可以跳转
3. 输入 `..` 可以返回到上级目录

#### autojump 插件

官网：[autojump](https://github.com/wting/autojump)

智能跳转插件，需要额外安装

```
brew install autojump
```
autojump 智能跳转，通过 zsh 记录访问过的目录，方便快速进行目录跳转。

在 `plugins` 中启用插件，并配置，将配置信息写入 `.zshrc` 中：

```
# env of autojump
[ -f /usr/local/etc/profile.d/autojump.sh ] && . /usr/local/etc/profile.d/autojump.sh
``` 

重新加载：

```
source ~/.zshrc
```

此时，就可以使用 `j [路径短写]` 来快速跳转目录了

> 与 autojump 类似的插件，还有 oh-my-zsh 自带插件 `z`，无需额外安装，比较推荐。[详情](https://github.com/robbyrussell/oh-my-zsh/wiki/Plugins-Overview#fs-jumping)

#### zsh-completions 插件

官网：[Github 主页](https://github.com/zsh-users/zsh-completions)

自动补全插件，需要额外安装

```
brew install zsh-completions
```

在 `plugins` 中启用插件，并配置 `~/.zshrc`：

```
# env of zsh-completions
fpath=(/usr/local/share/zsh-completions $fpath)
```

有时，需要重新 build zsh 的 `.zcompdump`

```
rm -f ~/.zcompdump; compinit
```

#### zsh-autosuggestions 插件

官网：[Github 主页](https://github.com/zsh-users/zsh-autosuggestions)

自动建议插件，需要额外安装

```
brew install zsh-autosuggestions
```

输入命令时，终端会自动提示你接下来可能要输入的命令。自动建议的字体颜色可能和你终端的颜色相近，你可以将其改成其它的，比如设置为 blue：

```
export ZSH_AUTOSUGGEST_HIGHLIGHT_STYLE='fg=blue'
```

在 `plugins` 中启用插件，并配置 `.zshrc`：

```
# env of zsh-autosuggestions
source /usr/local/share/zsh-autosuggestions/zsh-autosuggestions.zsh
```

同样，需要 `source ~/.zshrc` 重启配置生效。

#### zsh-syntax-highlighting 插件

官网：[Github 主页](https://github.com/zsh-users/zsh-syntax-highlighting)

语法高亮插件，需额外安装

```
brew install zsh-syntax-highlighting
```

在 `plugins` 中启用插件，并配置 `.zshrc` ：

```
# env of zsh-syntax-highlighting
source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

同样，需要 `source ~/.zshrc` 重启配置生效。

## iTerm2

官网：[iterm2](https://www.iterm2.com/)

iTerm2 一个用来取代系统终端的命令行工具。

推荐安装方式：

```
brew cask install iterm2
```

### 常用快捷键

* 查看剪贴板历史：command + shift + h
* 查看历史命令：command + ;
* 切换全屏：command + enter
* 新建 Tab：command + t
* Tab 中垂直分屏：command + d
* Tab 中水平分屏：command + shift + d

### 设置光标按照单词快速移动

设置 -> Profile -> Key -> Key Mappings 中修改 `option + ←` 和 `option + →` 的快捷映射为 `b` 和 `f`。

如图：

![1537867278019](/assets/img/media/1537867278019.jpg)

![1537867332441](/assets/img/media/1537867332441.jpg)

![1537867336766](/assets/img/media/1537867336766.jpg)

此时，使用 `Option + ←` 和 `Option + →` 即可使光标按照单词快速移动

### 复用上个会话的目录

设置 - Profiles - Working Directory - 选中 Reuse previous session’s directory

![1537868295567](/assets/img/media/1537868295567.jpg)

### Finder 插件 - termhere

官网：[TermHere](https://hbang.ws/apps/termhere/)

TermHere 可以帮用户在 Terminal 终端里快速进入『目标文件夹』的路径，以此快速操作文件，只需在 Finder 里的『目标目录』工具栏上选择『New Terminal Here』，即可快速调用 Terminal 终端，并进入目标目录的路径。

![yv47s](/assets/img/media/yv47s.jpg)

## tmux

官网：[tmux](https://github.com/tmux/tmux/wiki)

一个终端复用软件，可将终端方案化。经常使用服务器的话，可以考虑该工具。

安装：

```
brew install tmux
```