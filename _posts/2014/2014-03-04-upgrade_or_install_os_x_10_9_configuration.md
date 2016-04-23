---
layout: post
title: "升级或安装 OS X 10.9 以后的配置"
date: 2014-03-04 23:22
author: scsidisk
categories: MacOSX
---

####1. 安装 Xcode 命令行工具

在终端中运行 run xcode-select --install 点击安装即可.

####2. 重新安装 Homebrew 包

升级或重新安装系统以后 brew 安装的工具需要升级或重新安装:

```
brew uninstall $PACKAGE && brew install $PACKAGE
```

####3. 安装 GCC 4.2

一些软件需要gcc，不安装的话 bundle install or bundle upgrade
可能会出现一些错误. 重新安装 GCC 4.2 步骤如下:

```
brew tap homebrew/versions && brew install apple-gcc42
## 如果出现不能创建连接问题需要如下操作
brew link --force apple-gcc42
ln -nsf $(which gcc-4.2) /usr/bin/gcc-4.2
```

或者使用下面的批处理文件（注意备份数据文件）

```
#!/bin/sh

pkgs=`brew list`

for pkg in ${pkgs[*]}
do
   echo "reinstall ${pkg} ===================="
   #brew uninstall ${pkg}
   brew remove --force ${pkg}
   brew install ${pkg}
   cd ..
   echo "\n"
done
```