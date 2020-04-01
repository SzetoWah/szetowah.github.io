---
title: "Mac Python 入门"
date: 2018-10-25
draft: false
categories: ['技术']
---

### Mac 安装 Python

Mac 系统自带 Python 2.7，而 Python 3 早已面世良久。众所周知，Python 3 在设计的时候没有考虑向下兼容，所以很多使用 Python 2 开发的程序无法在 3.0 版本上运行。实际使用过程中，往往会不同程度地使用到 Python 2、3，故而此处记录两个版本安装共存的安装过程。

#### 安装 Homebrew

[Homebrew - 官网](https://brew.sh/index_zh-cn)

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

#### Python 2

查看系统 Python：

```
# 查看 python 命令路径
$ which python
/usr/bin/python

# 查看 python 版本
$ python -V
Python 2.7.10

# 第一次使用 python
$ python
Python 2.7.10 (default, Feb 22 2019, 21:55:15)
[GCC 4.2.1 Compatible Apple LLVM 10.0.1 (clang-1001.0.37.14)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>

# 使用 Ctrl + D 或 exit() 退出
```

再通过 Homebrew 查看 Python 的最新版本，以决定是否升级：

```
# 搜索 python
$ brew search python
==> Formulae
app-engine-python        boost-python3            gst-python               ipython@5                python                 python-yq                wxpython
boost-python             boost-python@1.59        ipython                  micropython              python-markdown          python@2               zpython

==> Casks
awips-python                                              kk7ds-python-runtime                                      mysql-connector-python

If you meant "python" specifically:
It was migrated from homebrew/cask to homebrew/core.

# 查看 python@2 版本信息
$ brew info python@2
python@2: stable 2.7.16 (bottled), HEAD
......

# 查看 python 版本信息
$ brew info python
python: stable 3.7.4 (bottled), HEAD
......
```

通过对比，可以看出，系统自带 python 版本比较旧，且由于 Mac 系统保护的原因，默认的 Python 无法对 PIP 一些包升级。因此选择使用 brew 库中的 `python@2` 进行替代。

安装最新版 python 2：

```
$ brew install python@2
```

#### Python 3

安装最新版 python 3：

```
$ brew install python
```

#### pip

pip 是 Python官方推荐的包管理工具，其本身属于 Python 的一部分。

检测是否安装 pip：

```
$ pip
# 如果提示报错，则表示没有安装
```

为 Python 2 安装 pip：

```
$ sudo easy_install-2.7 pip 
```

为 Python 3 安装 pip：

```
$ sudo easy_install-3.7 pip
```

查看 pip 版本：

```
$ pip -V
$ pip --version
pip 19.1.1 from /usr/local/lib/python3.7/site-packages/pip (python 3.7)
```

#### 解决版本共存

愿景：

* `python` - 使用 python3
* `python2` - 使用 brew 中安装的 python2
* `python3` - 使用 brew 中安装的 python3
* `pip` - 使用 python3 版本 pip
* `pip2` - 使用 python2 版本 pip
* `pip3` - 使用 python3 版本 pip

操作过程：

```
# 移除名为 python 指向 brew 版 python2.7 的软链接
$ sudo rm -rf /usr/local/bin/python

# 建立名为 python2 指向 brew 版 python2.7 的软链接
$ sudo ln -s /usr/local/Cellar/python@2/2.7.16/Frameworks/Python.framework/Versions/2.7/bin/python2.7 /usr/local/bin/python2

# 建立名为 python3 指向 brew 版 python3.7 的软链接
$ sudo ln -s /usr/local/Cellar/python/3.7.4/Frameworks/Python.framework/Versions/3.7/bin/python3.7 /usr/local/bin/python3

# 建立名为 python 指向 brew 版 python3.7 的软链接
$ sudo ln -s /usr/local/Cellar/python/3.7.4/Frameworks/Python.framework/Versions/3.7/bin/python3.7 /usr/local/bin/python
```

如需更改 python 默认版本为 Python 2.7，重新建立相对应版本的软链接并重置 pip 即可：

```
# 移除 python 当前版本软链接，建立名为 python 指向 brew 版 python2.7 的软链接
$ sudo rm -rf /usr/local/bin/python
$ sudo ln -s /usr/local/Cellar/python@2/2.7.16/Frameworks/Python.framework/Versions/2.7/bin/python2.7 /usr/local/bin/python

# 重置 pip，使其指向 pip2
$ sudo easy_install-2.7 pip
```

切换 Python 版本的过程比较繁琐，可以考虑将其放入 `~/.zshrc` 函数中：

```
# switch python version
function switch2python2() {
  sudo rm -rf /usr/local/bin/python
  sudo ln -s /usr/local/Cellar/python@2/2.7.16/Frameworks/Python.framework/Versions/2.7/bin/python2.7 /usr/local/bin/python
  sudo easy_install-2.7 pip
  echo -e "\n当前 Python 版本："
  /usr/local/bin/python -V
  echo -e "当前 Python pip 版本："
  /usr/local/bin/pip -V
}
function switch2python3() {
  sudo rm -rf /usr/local/bin/python
  sudo ln -s /usr/local/Cellar/python/3.7.4/Frameworks/Python.framework/Versions/3.7/bin/python3.7 /usr/local/bin/python
  sudo easy_install-3.7 pip
  echo -e "\n当前 Python 版本："
  /usr/local/bin/python -V
  echo -e "当前 Python pip 版本："
  /usr/local/bin/pip -V
}
```

更改完，记得 `source ~/.zshrc` 使配置生效。

至此，当需要系统默认 Python 版本为 2.7 时，我们只需执行 `switch2python2` 即可；同样，`switch2python3` 可快速切换系统默认 Python 版本为 3.7

#### 后记

实际开发过程中，可能还需要其他版本的 python，或者需要频繁切换 python 版本来进行测试开发，可以尝试使用 [pyenv](https://github.com/pyenv/pyenv) 这样更加优雅的 Python 版本管理工具。无需像上述记录的那样频繁更改软链接和重置 pip 来达到切换版本的目的。

安装 pyenv：

```
$ brew install pyenv
```

在 `~/.zshrc` 中添加环境变量（可选，一般安装时会自动添加）：

```
# pyenv
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
# (防止 brew doctor 报警)
alias brew="env PATH=${PATH//$(pyenv root)\/shims:/} brew"
# pyenv
if which pyenv > /dev/null;
  then eval "$(pyenv init -)";
fi
```

记得 `source ~/.zshrc` 使配置生效。

查看 pyenv 版本：

```
$ pyenv -v
```

更新 pyenv：

```
$ brew upgrade pyenv
```

一些常用命令：

<style>
table th:nth-of-type(1) {
    width: 200px;
    white-space: nowrap;
}
</style>

|命令|描述|
|----|----|
|pyenv commands|查看所有 pyenv 命令|
|pyenv versions|查看已安装 Python 版本|
|pyenv version|查看当前使用的 Python 版本（显示配置文件路径）|
|pyenv --version|查看当前使用的 Python 版本|
|pyenv install -l|查看可安装的 Python 版本|
|pyenv install \<version\>|安装指定版本 Python|
|pyenv uninstall \<version\>|卸载指定版本 Python|
|pyenv global \<version\>|设置全局的 Python 版本，全局设置 Python 版本为指定版本，会把版本号写入到 ~/.pyenv/version 文件中|
|pyenv local \<version\>|设置 Python 本地版本，设置当前路径下 Python 版本为指定版本，会在当前目录创建 .python-version 文件，优先级高于 global，–unset 参数可以用于取消当前设定|
|pyenv shell \<version\>|为当前 shell 会话设置 Python 版本，优先级比 global、local 都高，–unset 参数可以用于取消当前设定|
|pyenv shell --unset <br> pyenv local --unset|重置版本设置，只有 pyenv shell 和 pyenv local 命令有--unset参数|
|pyenv rehash|创建垫片路径(为所有已安装的可执行文件创建 shims，如：~/.pyenv/versions/*/bin/*，因此，每当你增删了 Python 版本或带有可执行文件的包（如 pip）以后，都应该执行一次本命令)|

* pyenv-virtualenv

[pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv) 是 pyenv 的插件，为 pyenv 设置的 Python 版本提供隔离的虚拟环境，设置虚拟环境后，在当前目录下面安装的第三方库都不会影响其他环境

安装：

```
$ brew install pyenv-virtualenv
```

在 `~/.zshrc` 中添加环境变量（可选，一般安装时会自动添加）：

```
# pyenv-virtualenv
if which pyenv-virtualenv-init > /dev/null;
  then eval "$(pyenv virtualenv-init -)";
fi
```

记得 `source ~/.zshrc` 使配置生效。

使用：

```
# 从当前版本创建 virtualenv
## pyenv virtualenv 虚拟环境名
$ pyenv virtualenv xxx-3.6.3

# 指定版本创建 virtualenv
## pyenv virtualenv 版本号 虚拟环境名
$ pyenv virtualenv 3.6.3 xxx-3.6.3

# 查看已创建的 virtualenv
$ pyenv versions


# 激活和停用 virtualenv

## 手动激活
## pyenv activate 虚拟环境名
$ pyenv activate xxx-3.6.3
## 自动激活
## 使用 pyenv local 虚拟环境名
## 会把`虚拟环境名`写入当前目录的 .python-version 文件中
## pyenv local 虚拟环境名
$ pyenv local xxx-3.6.3

## 关闭激活
$ pyenv deactivate


# 删除现有virtualenv
## pyenv uninstall 虚拟环境名
$ pyenv uninstall xxx-3.6.3
```