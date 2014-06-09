---
layout: post
title: lastcomm：显示以前使用过的命令的信息
date: 2010-12-28 13:53
author: scsidisk
category: CentOS
tags: CentOS, Linux, Shell, 命令
Slug: lastcomm_display_previously_used_commands
---

作用：accton用来启动进程记录，这样就会把所有的命令都记录到一个指定的文件中，而lastcomm就是用来查看这个文件的，以方便系统管理。

用法：lastcomm [选项]... [文件]...

主要选项如下。

-strict-match：精确匹配每一列

--user name：只显示指定用户的命令记录。

--command name：只显示指定命令的记录。

--tty name：只显示在指定终端上运行的命令。

-f filename：指定一个命令记录文件来代替默认文件--acct。

--debug：打印其他内核信息。

-V,--version：打印版本。

-h,--help：打印概要和系统默认统计文件（Linux里面的默认文件多是/var/log/pacct
和/var/account/pacct）。

应用实例如下。

显示曾经执行过的命令，如图4-1所示。

\# lastcomm -f /var/log/pacct

[![101337443](/images/2010/12/101337443-300x137.jpg)](/images/2010/12/101337443.jpg)

（点击查看大图）图4-1 显示曾经执行过的命令

每一项包含如下的信息。

进程的命令。

标志，由系统标准统计进程完成。

S：命令由超级用户运行。

F：命令由子进程运行，没有使用exec的子进程。

C：命令运行在PDP-11兼容模式下。

D：命令终止时产生core文件。

X：命令由信号SIGTERM终止。

运行命令的用户名。

进程使用的系统时间。

<div class="posttagsblock">
</div>

