---
layout:     post
title:      "CentOS 常用软件"
subtitle:   "CentOS Software"
date:       2017-07-12
author:     "StuartHua"
header-img: "img/post-bg-centos.jpg"
catalog: true
tags:
    - Notes
    - centos
---

## CentOS 常用软件

此文记录 centos 使用过程中，常用的软件安装

## git

CentOS 7.3 的 yum base repository 提供的 git 版本只有到 1.8.3，沒辦法使用 git 2 的一些新功能。若要安裝 git 2 但不想自己 compile 原始碼，想用 yum 來管理的話，可以使用 IUS 提供的 git2u 套件，安裝方式如下：

1. 若本來已有安裝 git，須先將之移除：
    ```
    $ sudo yum remove git
    ```
2. 安裝 epel-release repository 以免找不到必要的相依套件：
    ```
    sudo yum install epel-release
    ```
3. 安裝 IUS repository (for RHEL/CentOS 7)：
    ```
    $ sudo yum install https://centos7.iuscommunity.org/ius-release.rpm 
    ```
4. 安裝 git2u：
    ```
    $ sudo yum install git2u
    ```

提示：如果使用 CentOS 6 有同样的需求，只要把第 3 步网址中的 centos7 改成 centos 6 即可

## oh-my-zsh

### 安装 zsh 

```
yum install zsh
```

切换 shell 至 zsh

```
chsh -s /bin/zsh
```

查看安装了哪些shell

```
chsh -l
# or
cat /etc/shells
```

当前使用的shell

```
echo $SHELL
```

### 安装 oh-my-zsh

参考 [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh#via-curl)

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

### 安装 oh-my-zsh 插件：

在 `.zshrc` 文件中，启用插件：

```
plugins=(
  git autojump zsh-syntax-highlighting zsh-autosuggestions zsh-completions
)
```

* 安装 autojump

```
yum install autojump
yum install autojump-zsh
```

* 安装 zsh-syntax-highlighting

参考 [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)

```
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
```

更新 `.zshrc`

```
source $ZSH_CUSTOM/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

* 安装 zsh-autosuggestions

参考 [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)

```
git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
```

更新 `.zshrc`

```
source $ZSH_CUSTOM/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
```

* 安装 zsh-completions

参考 [zsh-completions](https://github.com/zsh-users/zsh-completions)

```
git clone https://github.com/zsh-users/zsh-completions.git $ZSH_CUSTOM/plugins/zsh-completions
```

更新 `.zshrc`

```
autoload -U compinit && compinit
```

rebuild zcompdump

```
rm -f ~/.zcompdump; compinit
```

执行更新

```
source ~/.zshrc
```

`.zshrc` 中 zsh 相关配置

```
# zsh-syntax-highlighting
source $ZSH_CUSTOM/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh

# zsh-autosuggestions
source $ZSH_CUSTOM/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh

# zsh-completions
autoload -U compinit && compinit
```

## shadowsocks-llb

使用repo安装shadowsocks-libev

```
# 下载源
wget https://copr.fedorainfracloud.org/coprs/librehat/shadowsocks/repo/epel-7/librehat-shadowsocks-epel-7.repo
mv librehat-shadowsocks-epel-7.repo /etc/yum.repos.d/

# 安装 ss
yum install epel-release -y   
yum install gcc gettext autoconf libtool automake make pcre-devel asciidoc xmlto c-ares-devel libev-devel libsodium-devel mbedtls-devel -y    
yum install shadowsocks-libev
```

配置文件

```
$ vim /etc/shadowsocks-libev/config.json

{
    "server":"你的服务器IP",
    "server_port":你的服务器端口,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"你的服务器密码",
    "timeout":300,
    "method":"aes-256-gcm"
}
```

启动代理：

```
nohup ss-local -c /etc/shadowsocks-libev/config.json < /dev/null &>> /var/log/ss-local.log &
```

关闭代理

```
pkill ss-local
```

查看代理是否运行

```
# 显示进程，则正在运行中
ps -aux | grep ss-local
```

可将相关命令放入 `.zshrc`

```
# shadowsocks-libev（记得替换用户名）
alias start-ss="nohup ss-local -c /etc/shadowsocks-libev/config.json >/home/【用户名】/log/shadowsocks-libev/sslocal.log 2>&1 &"
alias stop-ss="pkill ss-local"

function proxy() {
  export ALL_PROXY="socks5://127.0.0.1:1080"
  echo -e "已开启终端代理"
  curl ip.gs
}
function killproxy(){
  unset ALL_PROXY
  echo -e "已关闭终端代理"
  curl ip.gs
}
```

执行更新

```
source ~/.zshrc
```

### 使用

启用全局代理：

```
proxy
```

停止全局代理：

```
killproxy
```

## node (使用 nvm 安装)

参考 [nvm install script](https://github.com/creationix/nvm#install-script)

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
```

添加至 `.zshrc` 

```
# 键入内容
# nvm
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

# 更新环境
$ source ~/.zshrc
```

安装最新稳定版 node ：

```
# 查看可用的node版本
nvm ls-remote
# 安装某一版本 node
nvm install stable
# 根据版本号切换node版本
nvm use v8.11.3
# 设置默认的node版本
nvm alias default v8.11.3
```

查看已安装的node：

```
nvm list
# or
nvm ls
```

更新 nvm 版本

```
cd $NVM_DIR
git fetch
```

卸载 nvm

```
1. rm -rf $NVM_DIR
2. 删除 ~/.zshrc 中 nvm 的相关配置
```

安装 node 常用插件：

```
npm install pm2 -g
```

## ruby

参考 [RVM 安装](https://ruby-china.org/wiki/rvm-guide)

rvm 安装：

```
$ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
$ \curl -sSL https://get.rvm.io | bash -s stable
$ source ~/.bashrc
$ source ~/.bash_profile
```
修改 RVM 的 Ruby 安装源到 Ruby China 的 [Ruby 镜像服务器](https://cache.ruby-china.org/)，这样能提高安装速度

```
$ echo "ruby_url=https://cache.ruby-china.org/pub/ruby" > ~/.rvm/user/db
```

列出已知的 Ruby 版本

```
rvm list known
```

安装一个 Ruby 版本

```
rvm install 2.5.1
```

切换 Ruby 版本

```
rvm use 2.5.1
```

设置为默认版本

```
rvm use 2.5.1 --default
```

查询已经安装的ruby

```
rvm list
```

卸载一个已安装版本

```
rvm remove 1.8.7
```

安装 gem 常用插件：

```
gem install jekyll jekyll-paginate
```
