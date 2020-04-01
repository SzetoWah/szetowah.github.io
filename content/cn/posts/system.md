---
title: "操作系统的历史"
date: 2019-07-26
draft: false
categories: ['技术']
---

## 前言

一般来说，市面上常见的操作系统有 Windows、MacOS、Linux。实际上大多数操作系统可以被划分为两类: 

* 一类是，微软的基于 Dos 的 Windows NT 系统;
* 另一类是，“类Unix” 操作系统。这其中包括了 BSD，Linux，Mac OS X，Android，iOS，Chrome OS，PlayStaion 4 上运行的 Orbis OS，以及运行在路由器上的各种固件，这些操作系统通常都被称为 “类Unix” 操作系统

## Windows

DOS，是磁盘操作系统（英文：Disk Operating System）的缩写，是个人计算机上的一类操作系统。从 1981年 直到 1995年 的 15 年间，DOS 在 IBM PC 兼容机市场中占有举足轻重的地位。而且，若是把部分以 DOS 为基础的 Microsoft Windows版本，如Windows 95、98 和 Me 等都算进去的话，那么其商业寿命至少可以算到 2000年。

微软的 DOS 也成为众多 DOS 系统中最成功的一个。DOS 完全没有参照 Unix，这也是为什么 Windows 使用反斜杠划分文件路径，而其他所有系统都使用正斜杠。这个决定是在早期的 DOS 系统中做出的，而之后的 Windows 版本继承了这一设定，就像 BSD，Linux，Mac OS X，和其他类 Unix 操作系统继承了许多 Unix 设计因素一样。

Windows 3.1，Windows 95，Windows 98，和 Windows ME 都是建立在 DOS 的基础上。当时微软已经在开发一个更现代更稳定的操作系统，叫做 Windows NT－意思是 “Windows新技术”。Windows NT 最终通过 Windows XP 应用到普通计算机用户中，但是之前它都只用于针对企业用户的 Windows 2000 和 Windows NT。

如今所有微软的操作系统 都基于 Windows NT 内核。Windows 7，Windows 8，Windows RT，Windows Phone 8，Windows Server，以及 Xbox One 上的操作系统都在使用 Windows NT 内核。不像其他大多数操作系统那样，Windows NT 没有被开发成一个类Unix操作系统。

当然，微软也不是完全从零开始的。为了维护 DOS 和旧版本 Windows 软件的兼容性，Windows NT 继承了许多 DOS 里的设定，比如磁盘盘符，反斜杠区分文件路径，正斜杠作为命令行参数开关。

## 类 Unix

1969年，Unix 最早诞生于 AT&T 旗下贝尔实验室，到现在已经超过 40 个年头。在 70 年代末，Unix 出现两大主要阵营，分别是 System III&V 和 BSD。在这两大流派基础上，又分别上诞生了 IBM-AIX，HP-UX，Solaris, Apple Mac OS 和 Linux 等操作系统，所有这些操作系统又统称为 类-unix 或者 *nix。

下面介绍从 1969 年以来, Unix/Linux 相关主要事件。

* 1964年，贝尔实验室、麻省理工学院及美国通用电气公司共同研发了 Multics 操作系统。
* 1969年，MULTICS 因为过于复杂且工作进度过于缓慢而遭到裁撤，贝尔实验室退出 MULTICS 项目。
* 1971年，贝尔实验室的工程师们借鉴 MULTICS 发布 Unix 1.0（因为贝尔实验室是 AT&T（美国电话电报公司）的下属机构，所以 Unix 版权属于 AT&T）
* 1976年，Unix 6.0 发布（这个版本以后，出现 System III&V 和 BSD 两个主要派别）
* 1977年，伯克利加州大学学生 Bill Joy 借鉴 Unix，发布了类 Unix 系统 BSD
* 1979年，贝尔实验室发布最后一个可自由散布的 Unix 操作系统版本 Version 7，又称 V7。之后的 Unix 被 AT&T 公司商业化，并进行了闭源
* 1981年，Unix System III 发布
* 1982年，Sun OS 发布
* 1983年，Richard Matthew Stallman 创立 GNU 计划，旨在发展一个完全自由的类 Unix 操作系统，GNU 编写了很多自由免费的软件
* 1985年，基于 BSD 4.2 的 MIPS OS 发布
* 1985年，Apple Mac OS 的前身 GNU Mach（基于 BSD 4.3）发布
* 1987年，荷兰阿姆斯特丹自由大学的教授 Tanenbaum 借鉴 Unix V7 发布了没有使用 AT&T 公司源代码的类 Unix 系统 Minix，做为授课及学习之用。当时 Minix 只允许在教育上使用，不允许被用作任何商业场景，2000 年，Minix 以 BSD 许可协议重新发布，成为开源软件
* 1988年，SGI 发布基于 MIPS OS 的 IRIX
* 1990年，AT&T 起诉 BSD 侵犯了 Unix 的版权，这一起诉，对 BSD 打击极大，所有的开发活动都被迫停止，用户人心惶惶，担心自己也遭到 AT&T 的追究，因此 BSD 的使用急剧减少。直至 1994 年官司结束，BSD 又不幸发生分裂，变成了 FreeBSD、NetBSD 和 OpenBSD 三个版本，导致 BSD 直到今天，都还在操作系统的竞争中处在落后地位
* 1991年，芬兰学生 Linus Torvalds 不满足于 Minix 只能在教育上使用，借鉴 Minix 研发了类 Unix 系统 Linux。并加入 GNU 计划，使用 GNU 软件代替 MINIX 的软件，使得 Linux 以开源的形式得以快速发展
* 1992年，Sun Solaris 发布
* 1993年，NetBSD 和 FreeBSD 同时发布
* 1993年，Suse Linux 在德国发布
* 1993年，开源社区的 Debian 发布
* 1993年，开源社区的 Slackware 发布
* 1994年，Redhat Linux 在美国发布
* 1995年，基于 NetBSD 的 OpenBSD 发布
* 1998年，Mandrake在法国发布

![unix.jpg](https://raw.githubusercontent.com/stuarthua/PicGo/master/stuarthua.github.io/unix.jpg)

简单了解下历史，想要深究的话，可以查阅维基百科。

### 常见的类 Unix 操作系统

一般而言，常见的类 Unix 操作系统有

* 各种 Linux 发行版
* BSD
* MacOS

#### Linux 发行版

> 许多人所称的 “Linux” 实际上不是 Linux。

Linux 从技术上说只是 Linux 内核，典型的 Linux 发行版则包括了 Linux 内核和许多软件。这是为什么 Linux 有时被称为 GNU/Linux。事实上，许多在 Linux 上使用的软件同样也在 BSD 上使用。严格的说，Linux 只是一个内核。制作 Linux 发行版所要做的工作就是，汇集那些创建一个完整 Linux 操作系统所需的所有软件，将它组合成一个像 Ubuntu、Mint、Debian、RedHat 或者是 Arch 这样的 Linux 发行版。

Lunux 发行版众多，详细的列表可以参考 [维基百科 - Linux发行版列表](https://zh.wikipedia.org/wiki/Linux%E5%8F%91%E8%A1%8C%E7%89%88%E5%88%97%E8%A1%A8)，比较常见的划分方法是按打包方式划分：

* 基于 Dpkg（Debian系）
* 基于 RPM（Red Hat系）
* Slackware 系
* 其他打包方式的包

----

* 基于 Dpkg（Debian系）

Debian GNU / Linux 是一种强调使用自由软件的发行版。它支持多种硬件平台。Debian 及其派生发行版使用 deb 软件包格式，并使用 dpkg 及其前端作为包管理器。

> * `Debian GNU/Linux` - 最古老、最具影响力的 Linux 发行版，由 Debian 社区发布，强调开源和自由
> * `Ubuntu` - 基于 Debian，使用自己的软件包库，与 Debian 有所不同，旨在开发出更加友好的桌面
>   * `Linux Mint` - 基于 Ubuntu，人气与 Ubuntu 不相上下的发行版


* 基于 RPM（Red Hat系）

Red Hat Linux 和 SUSE Linux 是最早使用 RPM 格式软件包的发行版，如今 RPM 格式已广泛运用于众多的发行版。这两种发行版后来都分为商业版本和社区支持版本。Red Hat Linux 的社区支持版本现称为 Fedora，商业版本则称为 Red Hat Enterprise Linux（简称 RHEL）。

> * `Fedora` - 由 Red Hat 公司及其社区开发的 Linux 发行版
> * `Fedora` - 由 Red Hat 公司及其社区开发的 Linux 发行版
> * `CentOS` - 旨在100%地与 Red Hat Linux 企业版兼容，但不包含 Red Hat 的商业软件
> * `openSUSE` - 基于社区，完全开源，来自德国，是欧洲最流行的发行版之一。包括大量的软件

* Slackware 系

> 暂未涉猎

* 其他打包方式的包

> * `ArchLinux` - 基于 KISS 原则，针对 x86-64 的 CPU 做了优化，以 .pkg.tar.xz 格式打包并由包管理器进行跟踪维护，特别适合动手能力强的Linux用户
> * `Gentoo` - 采用自己独特的 Portage 包管理系统，吸引了许多狂热爱好者以及专业人士，由于能自己编译及调整源码依赖等选项，而获得至高的自定义性及优化的软件，在源码包也有相当多新旧版> 的选择，是个强调能自由选择的发行版

#### BSD

当前派生自 BSD 的类 Unix 操作系统可应用于多种硬件构架，比较常见的派生系统有：

* FreeBSD
  * FreeNAS
* NetBSD
  * OpenBSD

关于 BSD，更详细的内容可以查阅 [维基百科 - BSD](https://zh.wikipedia.org/wiki/BSD)。

#### MacOS

苹果公司推出的麦金塔（Macintosh）使用的是 MacOS（2011年及之前称 Mac OS X，2012年至2015年称OS X），严格意义上讲，MacOS 源于 BSD，其核心 Darwin，是以 BSD 源代码和 Mach 微核心为基础，由苹果公司和独立开发者社群合作开发。

Darwin（达尔文）于 2000年 有苹果公司发布给开放源代码社群，之后衍生出 OpenDarwin，可惜 OpenDarwin 由于多重原因影响于 2006年7月 终止。

## 一些常识 

### 关于 Windows、Mac OS、Linux

操作系统通常是和硬件相伴而生的，至少乔布斯这么认为。在乔布斯眼里，硬件和软件是一体的，只有完全兼容的硬件才能给用户带来良好的体验。所以苹果创立之初，便一直坚持软件硬件一起卖，也从没想要成为一家软件公司。从 20 世纪 80 年代起，乔布斯的苹果公司已经基本成型，和著名的 “蓝色巨人” IBM 公司鏖战正酣，疯狂地争夺着个人电脑这块蛋糕。此时，比尔·盖茨还只是一个软件开发商，并没有和这两大厂商叫板的可能，那时候也还没有 Windows，只有一个被称作 DOS 的低端系统，卡顿，操作复杂，在市场没有什么竞争力。

1981 年末，乔布斯邀请盖茨参观苹果计划推出的麦金塔（Macintosh，现多被简称为 Mac）样机，想让微软帮他开发与这款新机器相匹配的应用软件。比尔·盖茨被其图形界面和方便灵活的鼠标配合给吸引住了，在此之前他完全没有想到操作系统竟然可以做得这么简洁，决心开始开发自己的 Windows 操作系统。之后，微软在给苹果编写应用程序的同时，开始开发自己的 Windows 操作系统。但此时，Windows 还不足以对苹果的麦金塔系统构成威胁，于是微软决定将 Windows 的价格下降到 5 美元，这个超低的价格让很多用户连盗版都懒得装，由此 Windows 迅速崛起，占领了大片市场份额，在硅谷奠定了其霸主地位。

微软的 Windows 操作系统生意起步之后，迅速成长、蜕变，占据了全球大部分市场，微软从以前那个小小的软件供应商，成长为可以和苹果分庭抗礼的软件企业，但于此同时，比尔盖茨将编程开发这样一个建立在知识共享基础上的产物变成了完全商业化且高度闭源的公司资产，引起了一部分人的不满，所以之后有了很多程序员之间的鄙视链，很多自由软件开发者纷纷鄙视微软及其开发者。

在操作系统界，Unix 的光芒照耀世界。在 Unix 之后崛起的还有 Linus Benedict Torvalds 和开源精神。Unix 背后有贝尔实验室的大牛加持，自然性能无敌，但是后来其所有权几经辗转归属到 AT&T 旗下，作为一个商业公司产品，Unix 的价格也上升到 4000 美元，成为一款三思而远离的产品。面对 Unix 的闭源，陆续有了 Minix 及其后续作品 Linux。Linux 的开发者 Linus Benedict Torvalds 崇尚知识共享，选择开源 Linux，随后的开发者都遵循着 Linus 的开源精神，崇尚知识共享，公布源代码。最后有了 Linux 繁荣的今天。

值得一提的是，在 Unix 发展最初，其版权所有者 AT&T（美国电话电报公司）垄断了美国长途电话业务，所以美国司法部在 1958 年与它签了一个和解协议。AT&T同意不进入计算机业，不销售任何与计算机有关的产品，以避免司法部起诉它违反《反垄断法》，这意味着 AT&T 不能销售 Unix，任何要求得到源码的机构，都能免费得到，加州大学伯克利分校得到源码后，在 1979 年推出了一个自家的 Unix 版本，简称 BSD。之后 AT&T 因为被司法部起诉违反《反垄断法》，而被拆成 8 家小公司，司法起诉的同时，意味着 1958 年的和解协议失效，AT&T 从此可以进入计算机业，之后便有了 AT&T 对 BSD 侵犯 Unix 版权的诉讼，诉讼的官司持续了很久，且官司结束以后，BSD 又不幸发生分裂，变成了 FreeBSD、NetBSD 和 OpenBSD三个版本。这些原因导致 BSD 直到今天，都还在操作系统的竞争中处在落后地位。

### 关于类 Unix

从用户角度，更为关心的是目前使用的各种类 Unix 的相关性，下面介绍一些个人理解的常识：

* Unix 是商业操作系统的起点，后来的很多操作系统，包括我们常用的 Mac OS、在程序员圈子内颇受好评的 Linux 都直接或间接和 Unix 产生着联系
* 可以把 Unix 的派生系统大致分成两组：一组 Unix 派生系统是学术界开发的，如 BSD、MINIX；另一组是商用的 Unix 操作系统，如 AT&T UNIX，SCO UnixWare，Sun Microsystem Solaris，HP-UX，IBM AIX，SGI IRIX，Red Hat Enterprise Linux
* 严格讲来，MacOS 脱胎于 BSD，NeXTStep 基于最初版的 BSD 开发，苹果的 Mac OS X 基于 NeXTStep，iOS 基于 Mac OS X
* 运行在 PlayStation 4 上的 Orbis OS，也源于 BSD 操作系统
* Unix 系统大多是与硬件配套的，而 Linux 则可运行在多种硬件平台上
* Unix 是商业软件，而 Linux 是自由软件，免费、公开源代码的
* Linux 和 BSD 都是免费的，开源的，类 Unix 系统。他们甚至使用很多相同的软件
* 许多人所称的 “Linux” 实际上不是 Linux。Linux 从技术上说只是 Linux 内核，典型的 Linux 发行版则包括了 Linux 内核和许多软件。这是为什么 Linux 有时被称为 GNU/Linux。事实上，许多在 Linux 上使用的软件同样也在 BSD 上使用
* 严格的说，Linux 是只是一个内核。制作 Linux 发行版所要做的工作就是，汇集那些创建一个完整 Linux 操作系统所需的所有软件，将它组合成一个像 Ubuntu、Mint、Debian、RedHat 或者是 Arch 这样的 Linux 发行版。有许多不同的 Linux 发行版
* BSD 这个名字代表其内核和操作系统。例如，FreeBSD 提供了 FreeBSD 内核和 FreeBSD 操作系统。它是作为一个单一的项目维护的。换句话说，如果你想要安装 FreeBSD，就只有一个 FreeBSD 可供你安装。而如果你想要安装 Linux，你首先需要在许多 Linux 发行版之间选择。
* Linux 使用 GNU 通用公共许可证，即 GPL。如果你修改了 Linux 内核，并将其分发，你就必须放出您的修改的源代码。
* BSD 使用 BSD 许可证。如果你修改了 BSD 内核或发行版，并且发布它，你根本不需要必须发布其源代码。你可以自由地对你的 BSD 代码做任何你想做的事情，你没有义务发布的你修改的源代码，当然你想发布也行。
* Linux 和 BSD 都是开放源码的，但是以不同的方式。人们有时会陷入关于哪种许可证是 “更自由” 的辩论。GPL 可以帮助用户以确保他们可以拥有 GPL 软件的源代码，并限制开发人员迫使他们开放代码。BSD 许可证并不能确保用户可以拥有源代码，而是给开发人员选择是否公布代码的权利，即使他们想要把它变成一个闭源项目。

## 鸣谢

Thanks：

* [Unix 版权史](http://www.ruanyifeng.com/blog/2010/03/unix_copyright_history.html)
* [从 Windows 到鸿蒙 - 操作系统的前世今生](https://new.qq.com/omn/20190708/20190708A0O05T00.html)
* [Unix 发展历史和发行版本](http://www.ayshe.site/index.php?m=content&c=index&a=show&catid=138&id=9)
* [什么是 Unix 以及它为什么这么重要？](https://linux.cn/article-3452-1.html)
* [Linux 与 BSD 有什么不同?](http://blog.sae.sina.com.cn/archives/5164)