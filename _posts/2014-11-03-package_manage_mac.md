---
layout: post
title: 各种包管理器使用国内镜像加速下载
date: 2014-11-03
author: fdgogogo
category: MacOSX
tags: [MacOSX]
---

### 1. Python PyPI (pip)

添加豆瓣源

```
$ vim ~/.pip/pip.conf
添加

[global]
index-url = http://pypi.douban.com/simple
```

### 2. Node NPM

使用taobao源

```
npm config set registry https://registry.npm.taobao.org
npm info underscore （如果上面配置正确这个命令会有字符串response）
```

### 3. Ruby Gem

使用淘宝源

```
$ gem sources --remove https://rubygems.org/
$ gem sources -a http://ruby.taobao.org/
```

### 4. PHP Composer

尝试国内两个源以后，有时候会慢或找不到包。可以试用下面的源。

使用 Composer 需要提升速度问题：

- 升级PHP 版本到5.4以上
- 删除文件夹Vender（或者重命名），之后执行， 直接下载zip压缩包

```
php composer.phar install --prefer-dist
```

- 在composer.json文件中直接声明具体的bundle版本

```
$ composer config -g -e
添加

{
    "repositories": [
        { "packagist": false },
        {
            "type": "composer",
            "url": "http://composer-proxy.jp/proxy/packagist"
        }
    ]
}
```

转自：http://fduo.org/pip-gem-npm-chinese-mirror/
