---
layout: post
title: Linux下执行程序时发生错误 cannot restore segment prot after relocPermission denied
date: 2010-12-28 14:57
author: scsidisk
category: Linux
tags: [CentOS, Linux, Shell]
Slug: 2010-12-28-linux_error_runtime_cannot_restore_segment_prot_after_reloc
---

　　
原来这是SELinux搞的鬼，解决办法有两个

　　 1.
使用chcon
命令

　　
示例: chcon -t
texrel\_shlib\_t /usr/local/rsi/idl\_6.1/bin/bin.linux.x86/\*.so

　　 2.
禁止掉SELinux

　　
更改/etc/sysconfig/selinux
文件的内容为
SELINUX=disabled




