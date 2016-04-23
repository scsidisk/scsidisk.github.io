---
layout: post
title: "pgrep：查找匹配条件的进程"
date: 2010-12-28 13:53
author: scsidisk
categories: Linux
tags: CentOS, Linux, Shell
Slug: pgrep_find_matching_criteria
---

作用：pgrep命令用于查找当前运行的进程，并列出匹配给定条件的进程的PID。所有的条件都必须匹配才会被列出。

用法：pgrep[选项][程序名]

主要选项如下。

-l：列出程序名和进程ID。

-o：进程起始的ID。

-n：进程终止的ID。

应用实例如下。

用户cao查看sshd的进程列表：

[cao@localhost@cao]\$pgrep -l sshd

829 sshd
