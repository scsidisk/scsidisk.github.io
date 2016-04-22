---
layout: post
title: "硬盘安装 Linux ISO"
date: 2010-12-28 14:54
author: scsidisk
category: Linux
tags: [CentOS]
Slug: hard_disk_installed_linux_iso
---

通过GRUB（包括WINGRUB）命令行模式引导Linux的安装

在开机的时候，等GRUB画面出来，按c键进入命令行模式；如果您用的是WINGRUB，也有这样的模式，也按c键，道理是一样的；

在Linux和Windows中的GRUB，都有命令行的功能，这个功能极为有用，它不仅仅能引导系统，有时也能进行修复系统之用；再者就是引导安装Linux；

在安装系统的时候最好把linux iso复制到一个空的fat分区.并解压出 vmlinuz
initrd.img 两个文件.安装好grub for win 设置一下,重启
就按照下面的做.剩下的就跟平时安装一个样了.

grub for win 下载地址:http://sourceforge.net/projects/grub4dos/

举例：

比如
我们把vmlinz和initrd.img放在/dev/hda2中的fc5目录中；那GRUB的命令行应该怎么写呢？

grub\>kernel (hd0,1)/fc5/vmlinuz

grub\>initrd (hd0,1)/fc5/initrd.img

grub\>boot

如果直接放在/dev/hda3分区下，不放在任何目录中怎么应该写呢？

grub\>kernel (hd0,2)/vmlinuz

grub\>initrd (hd0,2)/initrd.img

grub\>boot

当然直接放在根分区也可以啦.

注意：这种方法不时候安装CentOS5.4-X86\_64，因为fat和fat32格式化的分区不能存放大于4G的文件。

<div class="posttagsblock">
</div>

