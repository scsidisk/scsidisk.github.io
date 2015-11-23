---
layout: post
title: 如何查看linux版本
date: 2010-12-28 15:23
author: scsidisk
category: Linux
tags: [CentOS, Linux, 版本]
Slug: how_to_view_the_linux_version
---

​1. 查看内核版本命令：

​1) [root@q1test01 \~]\# cat /proc/version

Linux version 2.6.9-22.ELsmp (bhcompile@crowe.devel.redhat.com) (gcc
version 3.4.4 20050721 (Red Hat 3.4.4-2)) \#1 SMP Mon Sep 19 18:00:54
EDT 2005

​2) [root@q1test01 \~]\# uname -a

Linux q1test01 2.6.9-22.ELsmp \#1 SMP Mon Sep 19 18:00:54 EDT 2005
x86\_64 x86\_64 x86\_64 GNU/Linux

​3) [root@q1test01 \~]\# uname -r

2.6.9-22.ELsmp

2.查看linux的版本主要有三种方法：

​1) 登录到服务器执行 lsb\_release -a ，即可列出所有版本信息，例如：

[root@3.5.5Biz-46 \~]\# lsb\_release -a

LSB Version: 1.3

Distributor ID: RedHatEnterpriseAS

Description: Red Hat Enterprise Linux AS release 4 (Nahant Update 1)

Release: 4

Codename: NahantUpdate1

[root@3.5.5Biz-46 \~]\#

这个命令适用于所有的linux，包括Redhat、SuSE、Debian等发行版。

​2) 登录到linux执行cat /etc/redhat-release ，例如如下：

[root@3.5.5Biz-46 \~]\# cat /etc/redhat-release

Red Hat Enterprise Linux AS release 4 (Nahant Update 1)

[root@3.5.5Biz-46 \~]\#

这种方式下可以直接看到具体的版本号，比如 AS4 Update 1

3）登录到linux执行rpm -q redhat-release ，例如如下

[root@3.5.5Biz-46 \~]\# rpm -q redhat-release

redhat-release-4AS-2.4

[root@3.5.5Biz-46 \~]\#

这种方式下可看到一个所谓的release号，比如上边的例子是2.4

这个release号和实际的版本之间存在一定的对应关系，如下：

redhat-release-3AS-1 -\> Redhat Enterprise Linux AS 3

redhat-release-3AS-7.4 -\> Redhat Enterprise Linux AS 3 Update 4

redhat-release-4AS-2 -\> Redhat Enterprise Linux AS 4

redhat-release-4AS-2.4 -\> Redhat Enterprise Linux AS 4 Update 1

redhat-release-4AS-3 -\> Redhat Enterprise Linux AS 4 Update 2

redhat-release-4AS-4.1 -\> Redhat Enterprise Linux AS 4 Update 3

redhat-release-4AS-5.5 -\> Redhat Enterprise Linux AS 4 Update 4

注意：第（2）（3）两种方法只对Redhat Linux有效

原文地址 http://bbs.eb2000.cn/redirect.php?tid=1928&goto=lastpost

<div class="posttagsblock">
[ ](http://technorati.com/tag/CentOS)

</div>

