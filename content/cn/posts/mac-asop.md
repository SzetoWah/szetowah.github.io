---
title: "Mac 10.14.6 编译 Android 9.0 源码"
date: 2019-07-25
draft: false
categories: ['技术']
---

### 前言

使用的硬件环境：

* Mac 10.14.6
* 500G 移动硬盘

Android 源码编译的流程：

* 源码下载
* 构建编译环境
* 编译源码
* 模拟器运行

附：

* 在 Android Studio 中查看源码

### 准备工作

一般而言，Android 源码占据大约 100G 的空间，然而编译后，极有可能再占据我们 150G 的空间，所以，在小容量的 Mac 上编译 Android 源码，使用移动硬盘自然而然成为了必然。

官网 - [Android 开放源代码项目 (AOSP)](https://android.googlesource.com/) 

#### 硬件及软件要求

硬件：

* Android 2.3 以上，需要使用 64 位系统编译，较低版本可以在 32 位系统中编译
* 拉取代码，至少需要 100G 磁盘空间，如果要进行编译，还需要 150G。如果要进行多次编译或使用 ccache，则需要更多空间
* 如果是在虚拟机中运行 Linux，则至少需要 16GB 的内存

软件：

* Python 2.6 - 2.7
* GNU Make 3.81 - 3.82
* Git 1.7 或更高版本
* Command_Line_Tools_macOS_10.13_for_Xcode_9.4 或更低版本

AOSP 的 `master` 分支都是在 Ubuntu (LTS) 版本中进行开发和测试，所以在其他系统上编译有一些特殊问题的话，切换到 Ubuntu 中应该可以解决。另外，AOSP 中 Android 的 master 分支带有预编译版本的 OpenJDK，无需额外安装；编译较低的版本则需要单独进行安装 OpenJDK。详见 [官方 - 要求](https://source.android.google.cn/source/requirements) 

#### 格式化移动硬盘

移动硬盘插到电脑后，格式化：

* 主引导记录
* EXFAT

这样做，可以使 Mac/Linux/Windows 都能识别并读取移动硬盘。这里，所使用的移动硬盘被命名为 `StuartDrive`，其路径即为：`/Volumes/StuartDrive`，后续我们在这个路径下创建磁盘映像。

#### 创建区分大小写的磁盘映像

Mac OS 默认会在不区分大小写的文件系统中运行，但由于 Git 并不支持此类文件系统，所以需要在 Mac OS 上建立一个区分大小写的磁盘分区。

有两种方法可以帮助我们创建磁盘映像

* 方法一：命令行
* 方法二：系统自带的磁盘工具

两种方法没有太大的差别，按照个人使用习惯创建即可，这里，使用命令行方式创建：

```
$ hdiutil create -type SPARSE -fs 'Case-sensitive Journaled HFS+' -size 200g /Volumes/StuartDrive/android.dmg
```

此时，在移动硬盘 `/Volumes/StuartDrive/` 目录下生成一个 200G 大小的`android.dmg.sparseimage` 文件，可简单理解为该文件是编译源码所需要的驱动盘。

如果后期还是觉得容量太小，可以使用 `resize` 来调整分区大小：

```
$ hdiutil resize -size 300g /Volumes/StuartDrive/android.dmg.sparseimage
```

#### 修改系统资源使用限制

编译过程十分漫长，可以视作是对机器性能的一个考验，合理的修改系统资源使用限制能在一定程度上帮助我们减少编译时间。

使用 `launchctl limit` 查看系统资源限制参数：

```
$ launchctl limit
	cpu         unlimited      unlimited
	filesize    unlimited      unlimited
	data        unlimited      unlimited
	stack       8388608        67104768
	core        0              unlimited
	rss         unlimited      unlimited
	memlock     unlimited      unlimited
	maxproc     709            1064
	maxfiles    256            unlimited
```

使用 `sysctl -a | grep kern.max` 查看系统硬件层次资源限制：

```
$ sysctl -a | grep kern.maxfiles
kern.maxvnodes: 132096
kern.maxproc: 1064
kern.maxfiles: 12288
kern.maxfilesperproc: 10240
kern.maxprocperuid: 709
kern.maxnbuf: 16384
```

得到上述参考结果后，使用 `ulimit -a` 可以查看当前的设置参数，再根据个人电脑的具体情况进行设定。

这里，进行如下设置，编辑 `~/.bash_profile`：

```
# ulimit settings
ulimit -S -n 10240
```
记得 `source ~/.bash_profile` 使配置生效。

> **注意：** 不要将 `ulimit` 的设置写在 `~/.zshrc` 文件中，编译中使用 `zsh` 可能出错，官方更推荐使用 `bash`.

### 源码下载

首先，装载我们新创建的区分大小写的磁盘映像，用于后续操作：

```
$ hdiutil attach /Volumes/StuartDrive/android.dmg.sparseimage -mountpoint /Volumes/android
```

此时，我们在 Finder 中，可以看到 `/Volumes/android` 已经装载成功。

>需要卸载的磁盘映像的话，则执行：`hdiutil detach /Volumes/android`

切换到 `/Volumes/android` 目录下，并且创建一个 `aosp` 的工作目录：

```
$ cd /Volumes/android/
$ mkdir aosp
$ cd aosp
```

稍后，我们会把 Android 源代码下载并保存到 `/Volumes/android/aosp` 目录。

> **注意：** 如果电脑中同时安装了多个版本 Python 的话，记得切换系统 Python 版本为 Python 2，使用 Python 3 的话，使用 repo 会报错。

#### 安装 Repo

Repo 是一款帮助我们在 Android 环境中更轻松地使用 Git 的工具。这里，没有使用官方推荐的方式下载按照 repo，而是使用 `Homebrew` 安装。使用帮助可以查看 [官方 - Repo 命令参考资料](https://source.android.google.cn/setup/develop/repo)。

```
$ brew install repo
```

#### 初始化 Repo 客户端

源地址的选择可以视个人网络情况而定，列出三种常用源，使用时记得要与后文中源码下载的源地址保持一致：

* 官方源：`https://android.googlesource.com/platform/manifest`
* 清华源：`https://aosp.tuna.tsinghua.edu.cn/platform/manifest`
* 科大源：`git://mirrors.ustc.edu.cn/aosp/platform/manifest`

如果对自己的网络环境足够自信，可以直接使用官方源下载源码（这会十分漫长，且意味着要翻墙下载超过 50G 的文件）。不想翻墙的话，可以跳过这一章节直接参考下一节中关于 `国内方法` 的介绍进行源码的下载。

在 `/Volumes/android/aosp` 目录下，运行 `repo init` 初始化，示例：

```
# 下载全部分支（默认）
$ repo init -u https://android.googlesource.com/platform/manifest

# 指定分支下载，-b 参数指定分支
$ repo init -u https://android.googlesource.com/platform/manifest -b android-9.0.0_r47
```

这里，选择指定分支 `android-9.0.0_r47`（附上 [官方分支列表](https://source.android.com/setup/build-numbers.html#source-code-tags-and-builds)，界面语言切换至英文后，可以看到全部版本，中文界面下版本可能显示不全）

初始化成功后，系统将显示一条消息，告诉我们 Repo 已在工作目录中完成初始化。客户端目录中现在应包含一个 `.repo` 目录，清单等文件将保存在该目录下。注意，这里不是下载源码，所以过程并不会很久。

#### 下载 Android 源码

参考：

* [官方教程 - 下载源代码](https://source.android.com/setup/build/downloading)
* [清华 - AOSP(Android) 镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/AOSP/)
* [科大 - AOSP(Android) 镜像使用帮助](https://lug.ustc.edu.cn/wiki/mirrors/help/aosp)

##### 官方方法

进入 `/Volumes/android/aosp` 目录，同步源码树（即下载更新源码，以后只需执行这条命令来同步）：

```
repo sync
```
同步过程是可能意外中断的，此时可以直接继续执行 `repo sync` 来继续同步，同步过程会下载大概 30G 的文件，耗时比较久。如果挂机很久，依旧没有下载成功的话，可以尝试使用以下脚本：

```
#!/bin/bash 
repo sync 
while [ $? -e 1 ]; do 
    repo sync 
done
echo success
```

保存脚本为 `download.sh`，运行：`sh download.sh` 即可，上述脚本会帮助我们在因为某些原因中断后，自动继续同步。

下载完成后会提示：

```
Syncing work tree: 100%(xxx/xxx), done.
```

##### 国内方法

此处介绍的方法是镜像站中比较推荐的 `下载每月初始化包` 的方式（一次性下载 aosp 版本控制信息仓库，而不需要耗费大量的时间缓慢地同步），故而可以忽略上一步中 `初始化 Repo 客户端` 的操作，直接删除 `aosp` 目录即可：

```
rm -rf Volumes/android/aosp
```

* 第一次下载

从 [清华大学开源软件镜像站 - aosp-monthly](https://mirrors.tuna.tsinghua.edu.cn/aosp-monthly/) 中选择最新版本进行下载。此处，选择最新文件 `aosp-latest.tar.md5`、`aosp-latest.tar`（这是一份打包的 AOSP 镜像，为一个 tar 包，只保留了 `.repo` 目录，下载后解包，再 `repo sync` 一遍即可得到完整的目录）进行下载，保存至 `/Volumes/android/` 目录。

先不着急解包，首先我们要做的是对比下载文件的 `checksum`，防止文件损坏导致的不能使用：

```
# 由于文件比较大，计算时间相对较久，耐心等待即可
$ md5 aosp-latest.tar
```

将运行结果与 `aosp-latest.tar.md5` 中记录的值进行对比，一致的话即可进行下一步解包。

解包我们下载的 `aosp-latest.tar` 文件至 `/Volumes/android/` 目录：

```
# 用于查看进度
$ brew install pv

# 解包
$ pv /Volumes/android/aosp-latest.tar | tar xf - -C /Volumes/android/
```

解包完成后，我们得到 `/Volumes/android/aosp` 目录，进入此目录，执行 `ll -a` 即可查看到隐藏目录 `.repo`，这里包含了我们需要到源码。

依旧不着急进行源码同步，先更改仓库源为 **科大源**（使用清华源同步也可以，但基于地域网络测试，科大源的下载速度比较好些，故而选择切换至科大源），并指定分支 `android-9.0.0_r47`：

```
# 使用 科大源 重新初始化仓库，并指定分支 android-9.0.0_r47
$ repo init -u git://mirrors.ustc.edu.cn/aosp/platform/manifest -b android-9.0.0_r47
```

如果遇到仓库源地址未更新的情况，可以尝试手动修改源地址：

```
## 附：手动修改源地址的方法 - 修改配置文件（共四处地方）
# 1. 修改 .repo/manifests.git/config
# 将 url 替换为 git://mirrors.ustc.edu.cn/aosp/platform/manifest

# 2. 修改 .repo/repo/config
# 将 url 替换为 git://mirrors.ustc.edu.cn/aosp/platform/manifest

# 3. 修改 .repo/manifests.git/.repo_config.json
# 将 remote.origin.url 替换为 git://mirrors.ustc.edu.cn/aosp/platform/manifest

# 4. 修改 .repo/repo/.repo_config.json
# 将 remote.origin.url 替换为 git://mirrors.ustc.edu.cn/aosp/platform/manifest
```

最后，使用 `repo sync` 进行代码同步：

```
$ repo sync
```

同步完成后，依旧会提示：

```
Syncing work tree: 100%(xxx/xxx), done.
```

* 之后的下载同步只需执行 `repo sync` 即可

### 构建编译环境

#### 切换 bash

如果电脑配置了 `zsh` 为默认 `shell`，使用 `zsh` 初始化编译环境时会提示可能导致异常：

```
WARNING: Only bash is supported, use of other shell would lead to erroneous results
```

建议切换至 `bash`，切换命令如下：

```
$ echo $SHELL
$ chsh -s /bin/bash
```

重启终端使其生效。

#### 环境设置

初始化环境：

```
$ source build/envsetup.sh
```

#### 选择编译目标

输入 `lunch`，会提示如下：

```
$ lunch 

You're building on Darwin

Lunch menu... pick a combo:
     1. aosp_arm-eng
     2. aosp_arm64-eng
     3. aosp_mips-eng
     4. aosp_mips64-eng
     5. aosp_x86-eng
     6. aosp_x86_64-eng

......
......
......

Which would you like? [aosp_arm-eng] 
```

这里列出了所有的可编译的目标。所有编译目标都采用 `BUILD-BUILDTYPE` 形式，其中 `BUILD` 是表示特定功能组合的代号，`BUILDTYPE` 是以下类型之一，表示在什么环境下运行：

|编译类型 (BUILDTYPE) |使用情况|
|----|----|
|user|权限受限；适用于生产环境|
|userdebug|与 “user” 类似，但具有 `root` 权限和可调试性；是进行调试时的首选编译类型|
|eng|具有额外调试工具的开发配置|

比如 `aosp_arm-eng` 的 `BUILD` 为 aosp_arm，`BUILDTYPE` 为 eng，aosp 代表 Android 开源项目，arm 表示系统是运行在 arm 架构的处理器上，更多见 [官方 - 选择设备编译系统](https://source.android.google.cn/source/running.html#selecting-device-build)。

如果有 Pixel 或 Nexus 真机，选择对应的编译目标即可。这里选择 x86_64 模拟器（支持 HAXM，可以使模拟器更快运行），即 `6. aosp_x86_64-eng`。在上面的 `Which would you like? [aosp_arm-eng]` 后面输入 6 即可。也可在运行时直接指定：

```
$ lunch 6
```

### 编译源码

在编译之前，通过输入以下命令，可以查看 Mac 的CPU核数：

```
$ sysctl -n machdep.cpu.core_count
4
```

上面输出的CPU核数为 4，则可以启动 4个 或 8个 线程来编译源码：

```
$ make -j4
```

接下来就是漫长的编译过程，一般需要 2~3 个小时，如果看到下面的提示，则表示已经编译完成：

```
#### build completed successfully (02:15:52 (hh:mm:ss)) ####
```

#### 遇到的问题

如果遇到编译失败，Google 得到解决方案准备尝试时，建议使用 `make clobber` 清除一下构建产物，然后重新进行编译。

* Could not find a supported mac sdk: [“10.10” “10.11” “10.12” “10.13”]

参考 [stackoverflow](https://stackoverflow.com/questions/50760701/could-not-find-a-supported-mac-sdk-10-10-10-11-10-12-10-13)、[cnblogs](https://www.cnblogs.com/larack/p/9646860.html)，编辑 `/Volumes/android/aosp/build/soong/cc/config/x86_darwin_host.go`，添加 `10.14` 的支持：

```
darwinSupportedSdkVersions = []string{
	"10.10",
	"10.11",
	"10.12",
	"10.13",
	"10.14",
}
```

执行 `make clobber` 清理上次构建，执行 `make -j4` 重新编译

* symbol(s) not found for architecture i386

参考 [Xcode 10 Release Notes](https://developer.apple.com/documentation/xcode_release_notes/xcode_10_release_notes)，Mac OS 自 10.14 开始，不在支持编译 32位程序。官方建议编译 32 位程序时使用 Xcode 9.4 或之前的版本。解决方法是使用降级的 `Command Line Tools`：

```
# 下载带有 32位 链接库的 CommandLineTools，推荐 Command_Line_Tools_macOS_10.13_for_Xcode_9.4
# 下载地址：https://download.developer.apple.com/Developer_Tools/Command_Line_Tools_macOS_10.13_for_Xcode_9.4/Command_Line_Tools_macOS_10.13_for_Xcode_9.4.dmg (需要 Apple ID 登录)
# 安装完成后，切换至降级版本的 Command Line Tools
$ sudo xcode-select -s /Library/Developer/CommandLineTools
```

执行 `make clobber` 清理上次构建，执行 `make -j4` 重新编译

> **注意：** 之后如需使用最新版 CommandLineTools，可以选择安装最新版 Xcode.app，使用其内置版本的 Command Line Tools，其目录为：/Applications/Xcode.app/Contents/Developer/Platform/MacOSX.platform/Developer/
> ```
> # 切换至最新版 CommandLineTools
> $ sudo xcode-select -r
> # 切换至降级版 CommandLineTools
> $ sudo xcode-select -s /Library/Developer/CommandLineTools
> ```

* Could not find command xz

安装缺失的指令：

```
brew install xz
```

### 模拟器运行

在编译完成后，终端里输入以下命令，便可使用模拟器来运行编译的版本：

```
$ set_stuff_for_environment
$ emulator
```

### 在 Android Studio 中查看源码

#### 编译 idegen 模块

上述编译完成后，运行以下命令，编译 `idegen` 模块：

```
$ mmm development/tools/idegen/
```

`idegen` 模块编译完成后会有如下提示：

```
#### make completed successfully (46 seconds) ####
```

继续运行以下命令，在 `aosp` 根目录生成对应的 `android.ipr`、`android.iml` IDEA 工程配置文件：

```
$ development/tools/idegen/idegen.sh

Read excludes: 17ms
Traversed tree: 218013ms
```

#### 导入源码

启动 Android Studio，选择一个已存在的 Android Studio 项目，然后选中 `aosp` 根目录下的 `android.ipr` 文件，如果提示需要 convert，确认即可。首次导入大概需要 20~30 分钟。

导入成功后，在 Project Structure 中，选择 `Modules`，删除所有的依赖，只保留 `<Modules source>`，`Android API Platform`。这样在查看源码跳转的时候，便不会进入 `android.jar`，而是直接跳转至本项目的文件中。

> **注意：** 导入源码时，选择全部导入，Android Studio 难免会比较卡，可以尝试在 `android.iml` 中进行修改，以排除一部分代码。

### 推荐

* [AOSP](https://source.android.com/)
* [Git repositories on android](https://android.googlesource.com/)
* [Android Revice](https://android-review.googlesource.com)
* [AndroidXRef](http://androidxref.com)

### 参考

* [AOSP](https://source.android.google.cn/)
* [AOSP(Android) 镜像使用帮助](https://lug.ustc.edu.cn/wiki/mirrors/help/aosp)
* [Mac OS下载、编译及导入Android源码](https://juejin.im/post/5cc5165fe51d456e781f2082#heading-12)
* [Mac OS 在移动硬盘上下载并编译Android源码](https://www.jianshu.com/p/55623ebeca4c)
* [macOS 系统编译 Android 8.1.0 源码全过程](http://tao93.top/2018/09/01/macOS%20%E7%B3%BB%E7%BB%9F%E7%BC%96%E8%AF%91%20Android%208.1.0%20%E6%BA%90%E7%A0%81%E5%85%A8%E8%BF%87%E7%A8%8B/)