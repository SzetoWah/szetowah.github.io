---
title: "关于 SSH keys"
date: 2018-07-20
draft: false
categories: ['技术']
---

在 [WIki - SSH keys (简体中文)](https://wiki.archlinux.org/index.php/SSH_keys_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)) 一文中，有详细介绍 SSH key 的一些概念及管理操作，以下，记录一些自己的理解及常用操作。

常用命令：

* ssh-keygen
* ssh-agent
* ssh-add

### 生成密钥对

`ssh-keygen` 选项说明（[man:ssh-keygen](https://man.linuxde.net/ssh-keygen)）： 

* -b：指定密钥长度；
* -e：读取openssh的私钥或者公钥文件；
* -C：添加注释；
* -f：指定用来保存密钥的文件名；
* -i：读取未加密的ssh-v2兼容的私钥/公钥文件，然后在标准输出设备上显示openssh兼容的私钥/公钥；
* -l：显示公钥文件的指纹数据；
* -N：提供一个新密语；
* -P：提供（旧）密语；
* -q：静默模式；
* -t：指定要创建的密钥类型。

#### 生成 rsa 默认密钥对

使用 `ssh-keygen` 命令生成密钥对

```
# 以 github 官方帮助文档为例
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

上例中，我们使用 `ssh-keygen` 生成了一对长度为 4096 bit (-b 4096) 的 rsa 加密的密钥对，描述 comment 为 `-C "your_email@example.com"`，密钥对默认存储在 `~/.ssh` 目录中

#### 如何选取合适对加密方式

从 `-t (type)` 结合 [RSA算法与DSA算法的区别](http://www.freeoa.net/osuport/sysec/algorithm-rsa-vs-dsa_3309.html) ，整理出 SSH 用到的加密方式：

* `RSA (Rivest-Shamir-Adleman, 非对称加密算法)` 是第一个公钥密码系统，广泛用于安全数据传输。它的安全性依赖于整数分解，因此永远不需要安全的RNG。与DSA相比，RSA更快地进行签名验证，但生成速度较慢。支持 768-16384 bit。关于理解对称加密和非对称加密，可以阅读 [白话解释 对称加密算法 VS 非对称加密算法](https://segmentfault.com/a/1190000004461428)，比较浅显易懂。
* `DSA (数字签名算法)` 是数字签名的联邦信息处理标准。它的安全性依赖于离散对数问题。支持 1024 bit。与RSA相比，DSA更快地生成签名，但验证速度较慢。如果使用了错误的数字生成器，可以打破安全性。自 OpenSSH 7.0 起，SSH默认情况下不再支持DSA密钥(ssh-dss)。以现在机算机的算力，DSA 1024-bit已经实际上可破解，不建议使用
* `ECDSA (椭圆曲线数字签名算法)` 能够提供与具有较小密钥的RSA相对相同级别的安全级别。它还有一个缺点，即DSA对不良RNG敏感。是ECC与DSA的结合，整个签名过程与DSA类似，所不一样的是签名中采取的算法为ECC。相对而言，生成的密钥更小，安全性更高。OpenSSH 5.7 建议默认使用 ECDSA，而一些较旧的 OpenSSH 版本可能不支持 ECDSA 密钥。
* `Ed25519(EdDSA - 爱德华兹曲线数字签名算法 的签名方案)` 使用 SHA-512/256 和 Curve25519，计算上比DSA轻，但区别在于除非你的机器具有很低的处理能力，否则很明显。在 oepnSSH 6.5 中推出

一般而言，应用最普遍的 RSA 经常出现在我们的视野中，且 RSA 既可以用于签名也可以用于加密(密钥交换)。

> **注意：** SSH 中，密钥对是用于认证的，选择更加复杂的密钥类型并不会在登录时加重本地电脑 CPU 的负担。

#### 选择密钥存储位置

输入 `ssh-keygen` 时，它会询问您将密钥对保存到何处、如何命名等。默认情况下，密钥对保存到 `~/.ssh` 下，可以使用 `-f` 来为即将生成的私钥命名并设置存储路径，如：

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com" -f ~/Desktop/iamkey
```

上例中，我们使用 `ssh-keygen` 生成了一对长度为 4096 bit (-b 4096) 的 rsa 加密的密钥对，描述 comment 为 `-C "your_email@example.com"`，私钥文件名为 `iamkey`，存储路径为 `~/Desktop/`。

注意查看生成的密钥对文件的权限，通常 私钥文件权限为 600，公钥文件权限为 644，这很重要，因为在服务器上配置时，很多人会忘记这一点。

#### 如何在不修改密钥对的情况下修改密码

在密钥对生成过程中，会提示我们输入密码，按照自己的记忆习惯设置后，使用了一段时间，我们可能会想要更改密码，当然，我们不会希望重新生成一对密钥对，因为一些其他的配置用到了我们的密钥对，我们不希望更改密钥对。

使用 `-p` 参数可以修改密钥对密码

```
$ ssh-keygen -f ~/Desktop/iamkey -p
```

#### 将公钥复制到远程服务器上

比较传统的做法是，`scp` 拷贝公钥文件到远程服务器，登录远程服务器，编辑 `~/.ssh/authorized_keys`，输入公钥内容。其实，可以尝试使用 `ssh-copy-id` 方便的复制公钥。此命令可以帮助复制公钥内容保存到服务器用户目录的 `~/.ssh/authorized_keys` 文件中。

```
$ ssh-copy-id -i ~/.ssh/id_bwg.pub -p 221 username@remote-server.org
```

#### SSH 目录文件及其权限

ssh 有关的目录及文件权限：

* `~/.ssh` - 设置 700 权限
* `~/.ssh/id_rsa`、`~/.ssh/authorized_keys`、`~/.ssh/config` - 设置 600 权限 
* `~/.ssh/id_rsa.pub`、`~/.ssh/known_hosts` - 设置 644 权限
* 阿里云密钥对 `aliyun-key-pair.pem` -  设置 400 权限（非服务器设置，在本地电脑处设置）

### 管理多组密钥对

平时，用户会使用 github、coding、阿里云 ECS，这些都有用到 ssh 的密钥对来认证身份。可以创建 `~/.ssh/config` 来管理多组密钥对，每一个 SSH 服务器对应一组密钥对，也可以多个 SSH 服务器公用一组密钥对。

#### 使用 SSH 登录阿里云 ECS：

```
# 通用配置
Host *
    ForwardAgent yes
    ServerAliveInterval 30
    ServerAliveCountMax 60
    TCPKeepAlive no
    ControlMaster auto
    ControlPath ~/.ssh/%h-%p-%r
    ControlPersist 4h
    Compression yes

# 阿里云 ECS
Host aliyun
    HostName 110.110.110.110
    User stuart
    Port 1602
    IdentityFile ~/.ssh/aliyun-key-pair.pem
```

在 `~/.ssh/config` 中添加上例内容后，我们便可以使用 `ssh aliyun` 以 stuart 用户的身份登录阿里云 ECS。无需重复指定端口、私钥、IP 地址。其效果等同于：

```
$ ssh -p 1602 -i ~/.ssh/aliyun-key-pair.pem stuart@110.110.110.110
```

通用配置是为了保持连接不因超时被关闭，参考 [保持SSH连接](http://einverne.github.io/post/2017/05/ssh-keep-alive.html)、`man ssh_config` 可加以理解。

至此，我们可以方便的管理多台服务器，而无需繁琐的记忆每个服务器的端口、地址等信息。

#### 使用 SSH 管理多个 github 账户：

假设，我们已经拥有一个默认的 gihub 账户 stuart，使用默认 id_rsa 私钥，除此之外，我们还拥有额外的两个 github 账户 dog、cat，分别使用 id_rsa_dog、id_rsa_cat 这两个不同的私钥。

在实际使用时，我们可能会偶尔忘记使用 `ssh-add` 加载相应的私钥，此时便可能会发生一些比较尴尬的报错，系统无法领会我们天马行空的意图，机械地使用默认加载的 `id_rsa` 私钥去 `clone` or `pull` dog/cat 账户中的项目。

```
# github account dog
Host dog.github
	Hostname github.com
	User git
	Identityfile ~/.ssh/id_rsa_dog
# github account cat
Host cat.github
	Hostname github.com
	User git
	Identityfile ~/.ssh/id_rsa_cat
```

在 `~/.ssh/config` 中添加上例内容，需求1: 我们希望使用默认 `id_rsa` 私钥来克隆默认 github 账户 stuart 的一个项目 OneProject: 

```
$ git clone git@github.com:stuart/OnePorgect.git
```

需求2: 我们希望使用 `id_rsa_dog` 私钥来克隆 github 账户 dog 的一个项目 DogPorgect：

```
$ git clone git@dog.github:dog/DogPorgect.git
```

需求3: 我们希望使用 `id_rsa_cat` 私钥来克隆 github 账户 cat 的一个项目 CatPorgect：

```
$ git clone git@cat.github:cat/CatPorgect.git
```

至此，我们可以使用不同的私钥来操作我们不同 git 平台的项目。

* 提示：  
  配置多个 Host 后，可以尝试使用类似 `ssh dog.github` 来验证连接，其效果等同于 `ssh git@github.com`，当然，使用 `-T` 参数来禁用伪终端得到的返回会更加简洁 `ssh -T git@github.com`。

### SSH-agent

如果在使用 `ssh-keygen`生成私钥的过程中，我们设置了密码，那么在每一次使用密钥进行登录的时候，系统都会提示我们输入密码，这确实比较繁琐。而 SSH agent 程序能够将我们已解密的私钥缓存起来，在需要的时候提供给 SSH 客户端。这样，我们就只需要将私钥加入 SSH agent 缓存的时候输入一次密码就可以了。经常使用 SSH 连接的话，这一点确实方便了很多。

需要知道的是，SSH agent 一般会设置成在登录会话的时候自动启动，并在整个会话中保持运行。有不少的 SSH agent 供我们选择：

* ssh-agent - OpenSSH 自带的一个 SSH agent，它可以直接作为 SSH agent 来使用，或者作为其他 SSH agent 的后端
* GnuPG - [gnupg](https://www.archlinux.org/packages/?name=gnupg)，也可以阅读 [阮一峰 - GPG入门教程](http://www.ruanyifeng.com/blog/2013/07/gpg.html) 一文加以了解

`ssh-agent` 选项说明（[man:ssh-agent](https://man.linuxde.net/ssh-agent)）：

* `-c`: 生成 C Shell 风格命令行
* `-s`: 生成 Bourne Shell 风格命令行
* `-d`: Debug模式
* `-k`: 杀死当前的 ssh-agent 进程
* `-t <seconds>`: 密钥在 ssh-agent 中的最大生命周期，单位秒

`ssh-add` 命令是把专用密钥添加到 `ssh-agent` 的高速缓存中。选项说明（[man:ssh-add](https://man.linuxde.net/ssh-add)）：

* -D：删除ssh-agent中的所有密钥.
* -d：从ssh-agent中的删除密钥
* -e pkcs11：删除PKCS#11共享库pkcs1提供的钥匙。
* -s pkcs11：添加PKCS#11共享库pkcs1提供的钥匙。
* -L：显示ssh-agent中的公钥
* -l：显示ssh-agent中的密钥
* -t life：对加载的密钥设置超时时间，超时ssh-agent将自动卸载密钥
* -X：对ssh-agent进行解锁
* -x：对ssh-agent进行加锁

#### ssh-agent 自动启动

通俗来讲，`ssh-agent` 就是一个密钥管理器，运行 `ssh-agent` 以后，使用 `ssh-add` 将私钥交给 `ssh-agent` 保管，其他程序需要身份验证的时候可以将验证申请交给 `ssh-agent` 来完成整个认证过程。

现在，我们可能想要 `ssh agent` 随终端 session 自动启动。因为我们不想每次在终端中输入 `git pull` 时，系统都提示我们加载私钥、输入私钥密码。这很繁琐。

在 [how-can-i-run-ssh-add-automatically-without-a-password-prompt](https://unix.stackexchange.com/questions/90853/how-can-i-run-ssh-add-automatically-without-a-password-prompt)、[adding-your-ssh-key-to-the-ssh-agent](https://help.github.com/en/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#adding-your-ssh-key-to-the-ssh-agent) 这两篇文章中，我们可以得到一些借鉴。

愿景：

* 启动终端时，开启 `ssh agent`，并加载私钥
* 打开多个会话 session，重用一个 `ssh agent`
* 退出终端后，再次启动终端，如果之前的 `ssh agent` 进程依然存在，重用之前的 `ssh agent`，无需重复开启
* 使用 ssh 验证时，无需重复输入私钥密码（如有设置）

`ssh agent` 的自动启动，编辑 `~/.zshrc` 或 `~/.bashrc`，添加以下内容：

```
# ssh-agent
if [ ! -S ~/.ssh/ssh_auth_sock ]; then
  eval `ssh-agent`
  ln -sf "$SSH_AUTH_SOCK" ~/.ssh/ssh_auth_sock
fi
export SSH_AUTH_SOCK=~/.ssh/ssh_auth_sock
# 有多个私钥需要添加的话，在末尾处添加
ssh-add -l > /dev/null || ssh-add ~/.ssh/id_rsa
```

当然，这会在 `~/.ssh` 目录下，生成 `~/.ssh/ssh_auth_sock` 文件，这是用于持久化的，记得不要删除即可。

#### ssh-agent 密码保存

解决了 `ssh agent` 的自动启动，我们现在来看私钥密码需要重复输入的问题。在 Mac 中，可以很方便地将密码保存到系统 keychain 里。编辑 `~/.ssh/config`，输入：

```
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_rsa
```

有可能会需要重启 macOS 本地的 ssh 服务来让 config 生效：

```
# 本地重启 macOS 中 sshd 服务
$ sudo launchctl stop com.openssh.sshd
$ sudo launchctl start com.openssh.sshd
```

储存其他私钥：

```
$ ssh-add -K ~/.ssh/id_coding_rsa
```

查看 keychain 中已存储的私钥：

```
$ ssh-add -A
```

同理，在 linux 系统中，也有类似的解决方案 -- [Keychain](http://www.funtoo.org/Keychain)