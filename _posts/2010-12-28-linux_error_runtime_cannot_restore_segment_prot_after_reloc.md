---
layout: post
title: Linux下执行程序时发生错误 cannot restore segment prot after relocPermission denied
date: 2010-12-28 14:57
author: scsidisk
category: CentOS
tags: CentOS, Linux, Shell
Slug: 2010-12-28-linux_error_runtime_cannot_restore_segment_prot_after_reloc
---

<span style="font-family: 'STHeiti Light';">　　</span>
<span style="font-family: 'STHeiti Light';">原来这是<span style="font: 12.0px Monaco;">SELinux</span>搞的鬼，解决办法有两个</span>

<span style="font: 12.0px STHeiti Light;">　　</span> 1.
<span style="font: 12.0px STHeiti Light;">使用</span>chcon
<span style="font: 12.0px STHeiti Light;">命令</span>

<span style="font: 12.0px STHeiti Light;">　　</span>
<span style="font: 12.0px STHeiti Light;">示例</span>: chcon -t
texrel\_shlib\_t /usr/local/rsi/idl\_6.1/bin/bin.linux.x86/\*.so

<span style="font: 12.0px STHeiti Light;">　　</span> 2.
<span style="font: 12.0px STHeiti Light;">禁止掉</span>SELinux

<span style="font: 12.0px STHeiti Light;">　　</span>
<span style="font: 12.0px STHeiti Light;">更改</span>/etc/sysconfig/selinux
<span style="font: 12.0px STHeiti Light;">文件的内容为</span>
SELINUX=disabled

<div class="posttagsblock">
[ ](http://technorati.com/tag/CentOS)

</div>

