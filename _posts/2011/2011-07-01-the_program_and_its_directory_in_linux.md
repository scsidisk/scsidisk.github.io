---
layout: post
title: Linux 中的程序及其目录
date: 2011-07-01
author: scsidisk
category: Linux
tags: Linux
---

本文主要是说明 `bin`, `sbin`, `usr/bin`, `usr/sbin`, `usr/local/bin`, `usr/local/sbin`, `su`, `su-`等等的区别。

一般情况下：

`/bin`: 系统的工具程序    
`/usr/bin`: 提供给管理员和一般用户使用的程序    
`/usr/local/bin`: 一般是用户安装的程序    

所有用户皆可用的系统程序放在 `/bin`    
超级用户才能使用的系统程序放在 `/sbin`    
所有用户都可用的应用程序放在 `/usr/bin`    
超级用户才能使用的应用程序放在 `/usr/sbin`    
所有用户都可用的与本地机器无关的程序存放在 `/usr/local/bin`    
超级用户才能使用的与本地机器无关的程序存放在 `/usr/local/sbin`    

这是一个默认的存放原则。s应该是代表superusr的含义吧。上面回复中有一点说的不明确。你如果在普通用户切换到超级用户root权限时，采用命令：

`su -`

注意，不是su，su后面还有`－`，那么就完全转入root的初始环境配置。也就不存在你所说的问题了。如果只是su，虽然可以转为root，但是shell的初始环境仍然为普通用户的当前环境设置。所以，推荐的方法是：


以普通用户身份登陆，按照默认的环境配置就可以，需要root权限的时候，执行su －。这样既安全，又方便。