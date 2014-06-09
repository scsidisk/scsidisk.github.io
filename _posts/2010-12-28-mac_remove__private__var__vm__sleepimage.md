---
layout: post
title: MAC去掉/private/var/vm/sleepimage
date: 2010-12-28 14:54
author: scsidisk
category: MacOSX
tags: Mac
Slug: mac_remove__private__var__vm__sleepimage
---

禁用MAC中的休眠，去掉/private/var/vm/sleepimage

执行如下命令即可以禁用MAC中的休眠，不会生成文件/private/var/vm/sleepimage文件：

\# pmset -a hibernatemode 0

\# rm -f /var/vm/sleepimage

察看状态

\# pmset -g

mode 0 不写硬盘，内存不断电

mode 1 写硬盘

mode 3 写硬盘，内存也不断电

<div class="posttagsblock">
</div>

