---
layout:     post
title:      "SSH Keys"
subtitle:   "SSH 密钥"
date:       2017-01-16
author:     "Stuart"
header-img: "img/post-bg-2017.jpg"
catalog: true
tags:
    - Notes
---

# SSH Keys
## 配置git用户名和邮箱

```
git config user.name "用户名"
git config user.email "邮箱"
```
在config后加上 --global 即可全局设置用户名和邮箱

## 生成sshkey

```
ssh-keygen -t rsa -C "xxxxx@xxxxx.com"
```
根据提示连续回车即可在~/.ssh目录下得到id_rsa和id_rsa.pub两个文件，id_rsa.pub文件里存放的就是我们要使用的key。

## 测试是否配置成功

```
ssh -T git@git.oschina.net
# or
ssh -T git@git.coding.net
# or
ssh -T git@github.com
# or
ssh -T git@bitbucket.org
```

## ssh-add

有时，Mac 重启后，ssh key 由于尚未加载，导致git连接失败，可以尝试

```
# 显示已加载的ssh key
ssh-add -l
# 加载公钥
ssh-add ~/.ssh/id_rsa
```

