---
layout: post
title: "linux如何修改hostname"
date: 2010-12-28 14:57
author: scsidisk
category: Linux
tags: [CentOS, hostname, Linux]
Slug: 2010-12-28-how_to_set_hostname_on_linux
---

很多人使用hostname 主机名
来修改,其实这个只是做为暂时的,重启后将恢复到原来的名字.

很多人说修改/etc/hosts文件,其实这个文件里的主机名只是为来提供给dns解析的.如果你用不上dns,只需要修改主机名,那修改这个没用.

其实是修改这个文件etc/sysconfig/network这个文件里的主机名.

NETWORKING=yes

HOSTNAME=主机名

记得重启!!!

----------

完整:

第一步：

\#hostname oratest

第二步：

修改/etc/sysconfig/network中的hostname

第三步：

修改/etc/hosts文件

出处：http://hi.baidu.com/sunshibing/blog/item/a4e6f8dec3fe835295ee374a.html
