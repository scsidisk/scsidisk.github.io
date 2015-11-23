---
layout: post
title: linux md5sum命令
date: 2010-12-28 15:22
author: scsidisk
category: Linux
tags: [CentOS, Linux, md5, Shell]
Slug: 2010-12-28-linux_md5sum_command
---

MD5算法常常被用来验证网络文件传输的完整性，防止文件被人篡改。MD5
全称是报文摘要算法（Message-Digest Algorithm
5），此算法对任意长度的信息逐位进行计算，产生一个二进制长度为128位（十六进制长度就是32位）的“指纹”（或称“报文摘要”），不同的文件产生相
同的报文摘要的可能性是非常非常之小的。

在Linux或Unix上，md5sum是用来计算和校验文件报文摘要的工具程序。一般来说，安装了Linux后，就会有md5sum这个工具，直接在命令行终端直接运行。

1、使用md5sum来产生指纹（报文摘要）命令如下：

md5sum file \> file.md5

或者

md5sum file \>\>file.md5

也可以把多个文件的报文摘要输出到一个md5文件中，这要使用通配符\*，比如某目录下有几个iso文件，要把这几个iso文件的摘要输出到iso.md5文件中，命令如下：

md5sum \*.iso \> iso.md5

2、使用md5报文摘要验证文件，方法有二：

把下载的文件file和该文件的file.md5报文摘要文件放在同一个目录下，然后用如下命令进行验证：

md5sum -c file.md5

然后如果验证成功，则会输出:正确

<div class="posttagsblock">
[ ](http://technorati.com/tag/CentOS)

</div>

