---
layout: post
title: "sleep：暂停进程"
date: 2010-12-28 13:53
author: scsidisk
categories: Linux
tags: CentOS, Shell, 命令
Slug: sleep_suspend_process
---

作用：sleep命令的功能是使进程暂停执行一段时间。

用法：sleep number [选项]

主要选项如下。

number：时间长度，后面可接s，m，h或d。

s：以秒为单位。

m：以分钟为单位。

h：以小时为单位。

d：以天为单位。

说明：如果没有指定时间，以秒为单位。此命令大多用于Shell程序设计，使两条命令执行之间停顿指定的时间。

应用实例如下。

使进程先暂停60秒，然后查看哪个用户登录到系统中：

＃sleep 60; who

<div class="posttagsblock">
</div>

