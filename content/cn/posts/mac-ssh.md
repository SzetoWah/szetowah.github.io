---
title: "Mac 高效使用指南（三）：SSH 的配置与管理"
date: 2018-09-03
draft: false
categories: ['技术']
---

SSH，安全外壳协议，一种加密的网络传输协议，可在不安全的网络中为网络服务提供安全的传输环境。SSH 最常见的用途是远程登录系统，人们通常利用 SSH 来传输命令行界面和远程执行命令。

## 拓展阅读

> `百度百科`：传统的网络服务程序，如：ftp、pop和telnet在本质上都是不安全的，因为它们在网络上用明文传送口令和数据，别有用心的人非常容易就可以截获这些口令和数据。而且，这些服务程序的安全验证方式也是有其弱点的， 就是很容易受到“中间人”（man-in-the-middle）这种方式的攻击。所谓“中间人”的攻击方式， 就是“中间人”冒充真正的服务器接收你传给服务器的数据，然后再冒充你把数据传给真正的服务器。服务器和你之间的数据传送被“中间人”一转手做了手脚之后，就会出现很严重的问题。通过使用SSH，你可以把所有传输的数据进行加密，这样"中间人"这种攻击方式就不可能实现了，而且也能够防止DNS欺骗和IP欺骗。使用SSH，还有一个额外的好处就是传输的数据是经过压缩的，所以可以加快传输的速度。SSH有很多功能，它既可以代替Telnet，又可以为FTP、PoP、甚至为PPP提供一个安全的"通道"。
>`维基百科`：SSH 也被指出有被嗅探甚至解密的漏洞。早在2011年，中国的互联网审查机构已经有能力针对SSH连线的刺探及干扰。而后爱德华·斯诺登泄露的文件也指出，美国国家安全局有时能够把SSH协议传输的信息解密出来，从而读出SSH会话的传输内容。2017年7月6日，非营利组织维基解密确认美国中央情报局已经开发出能够在Windows或Linux操作系统中窃取SSH会话的工具。

综上，境内使用 SSH 管理境外非法网站获益，理论上是透明的暴露在国家安全部门的监督范围内。

## 使用

通常，我们使用如下方式建立 ssh 连接：

```
ssh -p 54321 root@100.100.100.100
```

输入 root 密码后，便可以登录。

### 免密登录

`ssh-copy-id` 可以把本地主机的公钥复制到远程主机的 authorized_keys 文件上，同时，也会给远程主机的用户主目录（home）和 `~/.ssh`，和 `~/.ssh/authorized_keys` 设置合适的权限。

安装：

```
brew install ssh-copy-id
```

生成私钥公钥：

```
ssh-keygen -t rsa -b 4096 -C <your_email@example.com> -f <私钥公钥名>

# 如：
ssh-keygen -t rsa -b 4096 -C helloworld@app.com -f hello
```

将公钥上传致服务器（`~/.ssh/authorized_keys`）：

```
ssh-copy-id -i hello.pub root@100.100.100.100 -p 54321
```

### 配置别名

配置免密登录后，再次登录时，还需要输入端口号和服务器 IP，繁琐且难以记忆，我们可以考虑使用 SSH 的配置文件来实现别名登录。

全局 ssh 配置，编辑 `~/.ssh/config` 文件：

```
Host *
    ForwardAgent yes
    ServerAliveInterval 10
    ServerAliveCountMax 10000
    TCPKeepAlive no
    ControlMaster auto
    ControlPath ~/.ssh/%h-%p-%r
    ControlPersist 4h
    Compression yes
```

* ForwardAgent：假设我通过 SSH 连接上了服务器 A（通常在公司内部有跳板机时格外有用），又从服务器 A 连接到服务器 B，如果将 ForwardAgent 配置为 YES，在两台服务器之间传输数据就不会经过本机。
* ServerAliveInterval 和 ServerAliveCountMax：表示客户端定期向服务端发送心跳包，使得服务端不会断开 SSH 的连接。这里表示 10 秒发一次，发 1W 次。
* Contro***：每次建立远程连接，在 ~/.ssh 目录下都会建立一个 socket 文件。这三个配置连用，表示缓存 socket 文件，并保留 4 小时，这样可以加快下次连接的速度。

配置免密登录 VPS，编辑 `~/.ssh/config` 文件：

```
Host hellohost
    HostName 100.100.100.100
    User root
    Port 54321
    IdentityFile ~/.ssh/hello
```

这样，我们就为 IP 是 100.100.100.100，端口为 54321，用户名为 root 的远程主机设置了一个别名，然后就可以使用别名连接了：

```
ssh hellohost
```

### git 多用户

编辑 `~/.ssh/config` 文件：

```
# github 1
Host github.git1
	Hostname github.com
	User username1
	Identityfile ~/.ssh/id_rsa_1

# github 2
Host github.git2
	Hostname github.com
	User username2
	Identityfile ~/.ssh/id_rsa_2
```

测试 git 多用户配置：

```
ssh git@github.git1
# 正常，返回 username1

ssh git@github.git2
# 正常，返回 username2
```

### ssh-add 和 ssh-agent

在使用 git 拉取代码时，如果使用类似 `git@github.com:hello/FirstProject.git` 的仓库地址，通常需要我们事先将 ssh 私钥添加到 `ssh-agent` 中。

后台开启 ssh-agent：

```
$ eval "$(ssh-agent -s)"
> Agent pid 59566
```

添加 ssh 私钥：

```
$ ssh-add ~/.ssh/id_rsa
```

查看已加载的私钥：

```
$ ssh-add -l
```

删除某个特定加载的私钥：

```
$ ssh-add -d ~/.ssh/id_rsa
```

删除加载的全部私钥：

```
$ ssh-add -D
```

### 示例

一份 ssh config 示例：

```

Host *
    ForwardAgent yes
    ServerAliveInterval 10
    ServerAliveCountMax 10000
    TCPKeepAlive no
    ControlMaster auto
    ControlPath ~/.ssh/%h-%p-%r
    ControlPersist 4h
    Compression yes

## git 多用户

# github 1
Host github.git1
	Hostname github.com
	User username1
	Identityfile ~/.ssh/id_rsa_1

# github 2
Host github.git2
	Hostname github.com
	User username2
	Identityfile ~/.ssh/id_rsa_2

# aliyun
Host aliyun.git
	Hostname code.aliyun.com
	User username
 	Identityfile ~/.ssh/id_rsa

# coding
Host coding.git
	Hostname git.coding.net
	User username
 	Identityfile ~/.ssh/id_rsa

# oschina
Host oschina.git
	Hostname git.oschina.net
	User username
 	Identityfile ~/.ssh/id_rsa

# bitbucket
Host bitbucket.git
	Hostname bitbucket.org
	User username
 	Identityfile ~/.ssh/id_rsa

## ssh 服务器

# hellohost
Host hellohost
    HostName 100.100.100.100
    User root
    Port 54321
    IdentityFile ~/.ssh/hello
```