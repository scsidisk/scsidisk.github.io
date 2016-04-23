---
layout: post
title: "使用composer安装/更新PHP包"
date: 2014-03-17
author: scsidisk
categories: PHP
tags: PHP
---

composer是PHP用来管理PHP依赖关系的工具.

## 安装composer

```
curl -s https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
```

## 使用composer安装PHP包

比如安装monolog，

先建立/或更新composer.json:

```
{
	"require": {
		"monolog/monolog": "1.0.*"
	}
}
```

运行composer install即可安装monolog这个PHP包.

注：Packagist上面有很多现成的PHP包,要引用到项目中的话,都可以通过composer方式来安装使用.

## 调用已经安装的PHP包

composer方式安装的PHP包,默认都是安装到vendor目录下面,可以通过autoloading的方式来调用.

```
require 'vendor/autoload.php';

$log = new Monolog\Logger('name');
$log->pushHandler(new Monolog\Handler\StreamHandler('app.log', Monolog\Logger::WARNING));
$log->addWarning('Foo');
```

基本用法参考: http://getcomposer.org/doc/01-basic-usage.md

## 提升 PHP Composer 安裝及執行套件的速度

1. 指定下載包而非 Git 同步原始碼

設定："preferred-install": "dist"

如此則可以直接下載包（如 zip )，而非利用 Git 同步整個專案的原始碼歷史軌跡，節省一些安裝的時間。

2. 指定 HTTP 為傳輸方式

設定："github-protocols": ["http"]

倘若不在意安裝方式為 HTTP 非加密的傳輸協議，則可以使用這點來加速。

3. 優化 Class Map

執行命令：composer.phar dump-autoload –optimize

執行上述指令，可以令 composer 優化目前 class map 的設定。你可以前後比較 vendor/composer/autoload_classmap.php 的變化。

最後的設定

```
"config": {
	"preferred-install": "dist",
	"github-protocols": ["http"]
}
```

```
$ composer.phar dump-autoload --optimize
```
