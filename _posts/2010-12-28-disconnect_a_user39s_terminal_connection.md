---
layout: post
title: 断开某个用户的终端连接
date: 2010-12-28 14:54
author: scsidisk
category: Linux
tags: [CentOS, 命令]
Slug: disconnect_a_user39s_terminal_connection
---

断开某个用户的连接

who 查看用户连接

断开远程用户

fuser -k /dev/pts/x x为who下看到的这个用户的pts序号

断开本地用户

fuser -k /dev/ttyx x为who查看到的tty序号


