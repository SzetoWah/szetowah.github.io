---
title: "使用 SSH 免密登录阿里云"
date: 2018-10-01
draft: false
categories: ['技术']
---

### 前言

环境：

* 一台阿里云 ECS、CentOS 7 系统

愿景：

非 root 用户 stuart（此处以 stuart 为示例进行说明），使用 ssh 免密登录阿里云，并可以使用 `sudo su` 获取 root 权限操作服务器

### 操作过程

以下步骤，建议按照先后顺序执行，防止发生 `重启 sshd 服务后，无法登录` 的问题。

#### 创建新用户

一般而言，不推荐直接使用 root 登录和操作服务器，多数情况下，会新建一个或多个用户作为日常使用和维护。

##### 新建用户 stuart

登录[阿里云网页控制台](https://homenew.console.aliyun.com)，选中自己的实例 -> 远程连接，在网页控制台进行操作

```
$ adduser stuart
```

为新用户设置密码：

```
$ passwd stuart
```

更改管理员组 wheel 中的配置，允许获得 root 权限：

```
$ vi /etc/sudoers

// 红帽系的系统默认都有一个管理员组 wheel，一般建议把 sudo 用户添加到这个组
## Allows people in group wheel to run all commands
%wheel        ALL=(ALL)       ALL
```

赋予新用户 stuart 管理员权限：

```
$ usermod -aG wheel stuart
```

重新登录即可生效，使用 stuart 用户登录后，如需切换到 root 用户，只需：

```
# 此命令需要输入 stuart 用户的密码
$ sudo su
```

此时，我们已经拥有了日常操作用户 stuart，并可获得 root 权限管理服务器。但登录时，仍需要输入密码，较为繁琐。

#### 使用 SSH 密钥对

登录[阿里云网页控制台](https://homenew.console.aliyun.com)，参考 [官方文档 - 使用SSH密钥对](https://help.aliyun.com/document_detail/51793.html?spm=a2c4g.11186623.6.861.3d3d11c8D4PApU)，为自己的 ECS 创建并绑定 SSH 密钥对（注意，对密钥对的操作需要重启阿里云 ECS 实例才可生效）。

保存获得的证书文件 `aliyun-key-pair.pem` 于本地电脑 `~/.ssh` 目录下，设置其权限：

```
chmod 400 ~/.ssh/aliyun-key-pair.pem
```

##### 公钥设置

第一次配置密钥对，没有特别更改配置的情况下，在阿里云网页控制台操作并重启 ECS 后，即可使用 ssh 密钥对登录服务器。但有时，可能重启 ECS 实例后，依然不可以使用密钥进行登录，此时，需要我们手动将公钥设置到服务器 `~/.ssh/authorized_keys` 文件中。

查看公钥信息：

```
ssh-keygen -y -f /path_to_key_pair/my-key-pair.pem
```

返回公钥信息，类似如下所示：

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDdlrdZwV3+GF9q7rhc6vYrExwT4WU4fsaRcVXGV2Mg9RHex21hl1au77GkmnIgukBZjywlQOT4GDdsJy2nBOdJPrCEBIP6t0Mk5aPkK/fctNuKjcmMMOA8YUT+sJKn3l7rCLkesE+S5880yNdRjBiiUy40kyr7Y+fqGVdSOHGMXZQPpkBtojcV14uAy0yV6/htEqGa/Jq4fH7bR6CYQ2XgH/hCap29Mdi/G5Tx1nbUKuIHdMWOPvjGACGcXclex+lHtTGiAIRG1riyNRVC47ZEVCxxxxxx
```

>**说明** 如果该命令失败，请运行 `chmod 400 my-key-pair.pem` 命令更改权限，确保只有自己才能查看该文件。

登录[阿里云网页控制台](https://homenew.console.aliyun.com)，选中自己的实例 -> 远程连接，在网页控制台进行操作

默认是使用 root 用户登录，此处需要切换到 stuart 用户：

```
$ su stuart -
```

将上述公钥信息复制到 `/home/stuart/.ssh/authorized_keys` 文件（如果文件及目录提示不存在，手动创建即可）中：

```
$ mkdir ~/.ssh
$ chmod 700 ~/.ssh
$ vi ~/.ssh/authorized_keys
$ chmod 600 ~/.ssh/authorized_keys
```

##### 权限设置

> **说明** 如果 SSH 相关目录及文件的权限设置错误，可能导致无法登录服务器

```
$ chmod 700 ~/.ssh
$ chmod 600 ~/.ssh/authorized_keys
```

**注意**：与 ssh 有关的目录及文件权限：

* `~/.ssh` - 设置 700 权限
* `~/.ssh/id_rsa`、`~/.ssh/authorized_keys`、`~/.ssh/config` - 设置 600 权限 
* `~/.ssh/id_rsa.pub`、`~/.ssh/known_hosts` - 设置 644 权限
* `aliyun-key-pair.pem` -  设置 400 权限（非服务器设置，在本地电脑处设置）

#### 配置 sshd_config

root 用户使用密码在阿里云网页控制台登录 ECS

编辑 `/etc/ssh/sshd_config`（注意区分 `ssh_config`，`sshd_config`，后者是服务端配置）

清空初始设置：

```
$ echo > /etc/ssh/sshd_config
```

覆盖配置 `/etc/ssh/sshd_config`：

```
#	$OpenBSD: sshd_config,v 1.100 2016/08/15 12:32:04 naddy Exp $

# This is the sshd server system-wide configuration file.  See
# sshd_config(5) for more information.

# This sshd was compiled with PATH=/usr/local/bin:/usr/bin

# The strategy used for options in the default sshd_config shipped with
# OpenSSH is to specify options with their default value where
# possible, but leave them commented.  Uncommented options override the
# default value.

# If you want to change the port on a SELinux system, you have to tell
# SELinux about this change.
# semanage port -a -t ssh_port_t -p tcp #PORTNUMBER
#
#Port 22
#AddressFamily any
#ListenAddress 0.0.0.0
#ListenAddress ::

HostKey /etc/ssh/ssh_host_rsa_key
#HostKey /etc/ssh/ssh_host_dsa_key
HostKey /etc/ssh/ssh_host_ecdsa_key
HostKey /etc/ssh/ssh_host_ed25519_key

# Ciphers and keying
#RekeyLimit default none

# Logging
#SyslogFacility AUTH
SyslogFacility AUTHPRIV
#LogLevel INFO

# Authentication:

#LoginGraceTime 2m
#PermitRootLogin yes
#StrictModes yes
#MaxAuthTries 6
#MaxSessions 10

#PubkeyAuthentication yes

# The default is to check both .ssh/authorized_keys and .ssh/authorized_keys2
# but this is overridden so installations will only check .ssh/authorized_keys
AuthorizedKeysFile	.ssh/authorized_keys

#AuthorizedPrincipalsFile none

#AuthorizedKeysCommand none
#AuthorizedKeysCommandUser nobody

# For this to work you will also need host keys in /etc/ssh/ssh_known_hosts
#HostbasedAuthentication no
# Change to yes if you don't trust ~/.ssh/known_hosts for
# HostbasedAuthentication
#IgnoreUserKnownHosts no
# Don't read the user's ~/.rhosts and ~/.shosts files
#IgnoreRhosts yes

# To disable tunneled clear text passwords, change to no here!
#PasswordAuthentication yes
#PermitEmptyPasswords no
#PasswordAuthentication yes

# Change to no to disable s/key passwords
#ChallengeResponseAuthentication yes
ChallengeResponseAuthentication no

# Kerberos options
#KerberosAuthentication no
#KerberosOrLocalPasswd yes
#KerberosTicketCleanup yes
#KerberosGetAFSToken no
#KerberosUseKuserok yes

# GSSAPI options
GSSAPIAuthentication yes
GSSAPICleanupCredentials no
#GSSAPIStrictAcceptorCheck yes
#GSSAPIKeyExchange no
#GSSAPIEnablek5users no

# Set this to 'yes' to enable PAM authentication, account processing,
# and session processing. If this is enabled, PAM authentication will
# be allowed through the ChallengeResponseAuthentication and
# PasswordAuthentication.  Depending on your PAM configuration,
# PAM authentication via ChallengeResponseAuthentication may bypass
# the setting of "PermitRootLogin without-password".
# If you just want the PAM account and session checks to run without
# PAM authentication, then enable this but set PasswordAuthentication
# and ChallengeResponseAuthentication to 'no'.
# WARNING: 'UsePAM no' is not supported in Red Hat Enterprise Linux and may cause several
# problems.
UsePAM yes

#AllowAgentForwarding yes
#AllowTcpForwarding yes
#GatewayPorts no
X11Forwarding yes
#X11DisplayOffset 10
#X11UseLocalhost yes
#PermitTTY yes
#PrintMotd yes
#PrintLastLog yes
#TCPKeepAlive yes
#UseLogin no
#UsePrivilegeSeparation sandbox
#PermitUserEnvironment no
#Compression delayed
#ClientAliveInterval 0
#ClientAliveCountMax 3
#ShowPatchLevel no
#UseDNS yes
#PidFile /var/run/sshd.pid
#MaxStartups 10:30:100
#PermitTunnel no
#ChrootDirectory none
#VersionAddendum none

# no default banner path
#Banner none

# Accept locale-related environment variables
AcceptEnv LANG LC_CTYPE LC_NUMERIC LC_TIME LC_COLLATE LC_MONETARY LC_MESSAGES
AcceptEnv LC_PAPER LC_NAME LC_ADDRESS LC_TELEPHONE LC_MEASUREMENT
AcceptEnv LC_IDENTIFICATION LC_ALL LANGUAGE
AcceptEnv XMODIFIERS

# override default of no subsystems
Subsystem	sftp	/usr/libexec/openssh/sftp-server

# Example of overriding settings on a per-user basis
#Match User anoncvs
#	X11Forwarding no
#	AllowTcpForwarding no
#	PermitTTY no
#	ForceCommand cvs server

# 自定义配置（Port 推荐注释初始 22 端口的设置）
Port 1602
UseDNS no
PermitRootLogin no
PasswordAuthentication no
```

说明：

* Port - 指定 ssh 端口
* UseDNS - 用来控制在登录阶段是否执行客户端主机名查询。默认为"yes"，DNS 查询阶段超时，有可能导致登录慢
* RSAAuthentication - 是否允许使用纯 RSA 公钥认证。仅用于SSH-1。默认值是"yes"
* PubkeyAuthentication - 是否允许公钥认证。仅可以用于SSH-2。默认值为"yes"
* AuthorizedKeysFile - 存放该用户可以用来登录的 RSA/DSA 公钥，默认为".ssh/authorized_keys"
* PermitRootLogin - 是否允许 root 远程登录。默认为 "yes"
* PasswordAuthentication - 是否允许使用基于密码的认证。默认为"yes"

自定义 ssh 端口：

```
Port 1602
```

禁用 DNS 查询，加快连接登录速度：

```
UseDNS no
```

禁止 root 用户远程登录：

```
PermitRootLogin no
```

禁止使用密码登录：

```
PasswordAuthentication no
```

重启 sshd 服务：

```
systemctl restart sshd
```

退出并重新连接服务器，查看配置是否生效：

```
$ exit
$ ssh -p 1602 -i ~/.ssh/aliyun-key-pair.pem stuart@xxx.xxx.xxx.xxx
```

此时，我们禁用了密码登录、root 远程登录，仅提供 stuart 用户免密登录服务器，且 stuart 用户拥有管理员权限，可以进行日常管理。但每次登录，都需要指定端口号、证书、IP地址，仍然较为繁琐。

#### 本地电脑配置

编辑本地电脑 `~/.ssh/config` 设置别名登录：

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
    AddKeysToAgent yes

# 阿里云 ECS
Host aliyun
    HostName xxxxxx
    User stuart
    Port 1602
    IdentityFile ~/.ssh/aliyun-key-pair.pem
```

此时，只需 `ssh aliyun` 即可实现 stuart 用户免密登录，并拥有管理员权限，需要安装软件时，只需 `sudo su` 切换到 root 用户，进行安装即可。

#### 参考

* 参考 `man ssh_config` 查看更多配置项的涵义
* 参考 [保持SSH连接](http://einverne.github.io/post/2017/05/ssh-keep-alive.html)  设置通用配置
* 参考 [How to make ssh-agent automatically add the key on demand](https://superuser.com/questions/325662/how-to-make-ssh-agent-automatically-add-the-key-on-demand) 配置 AddKeysToAgent
* 参考 [Generating a new SSH key and adding it to the ssh-agent](https://help.github.com/en/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) 查看 mac、linux、windows、all 等各个平台下 ssh-key 的设置及添加