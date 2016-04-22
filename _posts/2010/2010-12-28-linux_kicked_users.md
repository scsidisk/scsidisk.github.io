---
layout: post
title: "linux踢出用户"
date: 2010-12-28 13:51
author: scsidisk
category: Linux
tags: [CentOS, Linux, Shell, SSH]
Slug: linux_kicked_users
---

> linux强制踢出用户命令：
>
> 一、输入w命令查看已登录用户信息
>
> [root@KW\_S01\_192.168.1.106\_A \~]\# w
>  19:22:31 up 2:11, 3 users, load average: 0.00, 0.00, 0.00
>  USER TTY FROM LOGIN@ IDLE JCPU PCPU WHAT
>  root pts/0 192.168.1.178 18:41 0.00s 0.16s 0.01s w
>  root pts/1 192.168.1.178 19:22 4.00s 0.14s 0.05s ssh localhost
>  root pts/2 localhost.locald 19:22 4.00s 0.07s 0.07s -bash
>
> 二、使用命令pkill -kill -t 用户tty
>  [root@KW\_S01\_192.168.1.106\_A \~]\# pkill -kill -t pts/2
>
> 三、验证操作是否成功
>  [root@KW\_S01\_192.168.1.106\_A \~]\# w
>  19:24:25 up 2:12, 2 users, load average: 0.00, 0.00, 0.00
>  USER TTY FROM LOGIN@ IDLE JCPU PCPU WHAT
>  root pts/0 192.168.1.178 18:41 0.00s 0.18s 0.02s w
>  root pts/1 192.168.1.178 19:22 1:58 0.09s 0.09s -bash
>
> 登陆用户信息说明：
>
> USER：显示登陆用户帐号名。用户重复登陆，该帐号也会重复出现。
>
> TTY：用户登陆所用的终端。
>
> FROM：显示用户在何处登陆系统。
>
> LOGIN@：是LOGIN AT的意思，表示登陆进入系统的时间。
>
> IDLE：用户空闲时间，从用户上一次任务结束后，开会记时。
>
> JCPU：一终端代号来区分，表示在摸段时间内，所有与该终端相关的进程任务所耗费的CPU时间。
>
> PCPU：指WHAT域的任务执行后耗费的CPU时间。
>
> WHAT：表示当前执行的任务。
>
> [From [<cite>linux踢出用户 - linux系统管理 - LINUX
> 点点滴滴</cite>](http://blog.chinaunix.net/u3/108043/showart_2121929.html)]

<div class="posttagsblock">
</div>

