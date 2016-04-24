---
layout: post
title: "Mac OS X 上面的仿真终端"
date: 2016-04-22
author: scsidisk
categories: MacOSX
tags: SecureCRT, Zoc, SSH
---

在 Mac 上面使用 SSH 链接服务器, 很方便。但是如果管理机器很多，就比较麻烦。

下面介绍三个软件，希望对大家有所帮助。

Mac 自带 Terminal
-----------------

管理海量 server 时，Terminal & alias & bash-completion 也不错, 具体的命令规则可以自己定义。

    $ brew install bash-completion
    $ ssh.nginx my.[tab]
    my.web01
    my.web02
    my.cache01
    my.store.02

    $ ssh.db work.db[tab]
    work.db.mysql01.master
    work.db.mysql02.slave
    work.db.redis01
    work.redis02

包括利用不同端口跳板到内网IP时，巨方便无比。最典型的：青云 VPS 的管理

SecureCRT
----------

这个软件大家在 Windows 下面使用比较多，SecureCRT for Mac 也不错，不过需要收费。

下载: <https://www.vandyke.com/download/securecrt/7.3/index.html>

如果不想购买，可以下载 securecrt_mac_crack.pl 文件生成 License . 下载地址请大家 google .

ZOC
-----

Zoc 也是 Mac 下面的一个很不错的软件，同样也可以记住密码。

下载 <http://www.emtec.com/downloads/zoc/>

可以下载 644 输入下面的注册码

    1st part: 51698/01027/34713
    2nd part: 00937

然后在升级到 7073， 如果不行，请重新安装 644 自己尝试升级到更高的版本。
