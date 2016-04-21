---
title: 如何使用 Homebrew 安装指定版本的工具？
layout: post
author: scsidisk
date: 2013-05-10 13:25:56
category: MacOSX
tags: [Mac, Brew, Homebrew]
---

Homebrew 是一个 Mac 下的安装管理 Unix 工具的工具。安装好之后，在命令行下使用 brew install FORMULANAME就可以安装 FORMULANAME 对应的工具，它会处理好依赖关系，非常方便。默认情况下，安装最新版本。

但是在某些情况下，我们可能需要安装“旧”版本的工具，或者说安装指定/特定版本的工具，该怎么办呢？还好，Homebrew 已经提供了这类的支持。

今天安装 gsl 这个 rubygem ，编译本地库时失败了。我机器的环境是：

```
gsl-1.15
ruby-1.9.3p125
```

其中 gsl 是使用 brew install gsl 安装的，安装了最新的1.15版本。执行 gem install gsl 时的一条错误信息是：

```
conflicting types for ‘gsl_matrix_complex_equal’
```

于是顺着这条错误信息 Google ，发现很多人都遇到这个问题了。有人说，应该安装 gsl-1.14 而不是 gsl-1.15 。而最新的 gem 版本为 1.14.7 。看来很可能是版本不兼容。

所以我需要给 gsl 降级。 由于 gsl 是通过 Homebrew 安装的，所以需要找到安装特定版本工具的方法。于是在 Stackoverflow 上找到了方法。其实很简单：

1. 查看 brew 支持哪些版本的 gsl

```bash
$ brew versions gsl
1.15     git checkout 164c57f /usr/local/Library/Formula/gsl.rb
1.14     git checkout 83ed494 /usr/local/Library/Formula/gsl.rb
1.13     git checkout b0b2584 /usr/local/Library/Formula/gsl.rb
```

非常幸运，1.14 包括在内。

2. 进入 brew 所在的git仓库

```bash
$ cd `brew --prefix`
```

3. 复制粘贴刚才 brew versions sql 命令的提示。执行

```bash
$ git checkout 83ed494 /usr/local/Library/Formula/gsl.rb
```

4. 此时安装使用 brew install gsl 会提示错误

```
Error: gsl-1.15 already installed
To install this version, first `brew unlink gsl'
```

因此需要先取消之前的链接，执行

```bash
$ brew unlink gsl
```

提示

```
Unlinking /usr/local/Cellar/gsl/1.15... 16 links removed
```

5. 安装成功

```bash
$ brew install gsl
```
