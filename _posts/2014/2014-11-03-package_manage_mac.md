---
layout: post
title: 各种包管理器使用国内镜像加速下载
date: 2014-11-03
author: fdgogogo
category: MacOSX
tags: MacOSX
---

### 1. Python PyPI (pip)

添加豆瓣源

```
$ vim ~/.pip/pip.conf

[global]
index-url = http://pypi.douban.com/simple 
```

### 2. Node NPM

使用cnpmjs

```
vim ~/.npmrc

registry = http://registry.cnpmjs.org
```

### 3. Ruby Gem

使用淘宝源

```
$ gem sources --remove https://rubygems.org/
$ gem sources -a http://ruby.taobao.org/
```

转自：http://fduo.org/pip-gem-npm-chinese-mirror/
