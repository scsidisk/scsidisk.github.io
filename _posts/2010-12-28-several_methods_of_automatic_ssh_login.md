---
layout: post
title: 自动ssh登录的几种方法
date: 2010-12-28 14:56
author: scsidisk
category: CentOS
tags: CentOS, SSH
---

​1. 自动ssh/scp方法==

A为本地主机(即用于控制其他主机的机器) ;  
B为远程主机(即被控制的机器Server), 假如ip为192.168.60.110;

A和B的系统都是Linux

在A上运行命令:  
\# ssh-keygen -t rsa (连续三次回车,即在本地生成了公钥和私钥,不设置密码)

\# ssh root@192.168.60.110 “mkdir .ssh” (需要输入密码)

\# scp \~/.ssh/id\_rsa.pub root@192.168.60.110:.ssh/id\_rsa.pub
(需要输入密码)

在B上的命令:  
\# touch /root/.ssh/authorized\_keys (如果已经存在这个文件, 跳过这条)

\# cat /root/.ssh/id\_rsa.pub \>\> /root/.ssh/authorized\_keys
(将id\_rsa.pub的内容追加到authorized\_keys 中)

回到A机器:  
\# ssh root@192.168.60.110 (不需要密码, 登录成功)

​2. 控制n个机器如上所述自动登录  
那就需要n对钥匙(密钥和公钥), ssh-keygen 命令可以随意更改钥匙对的名字,
比如:

\# ssh-keygen -t rsa

Generating public/private rsa key pair.

Enter file in which to save the key (/root/.ssh/id\_rsa):
/root/.ssh/id\_rsa\_192.168.60.110

这样私钥和公钥的名字分别就是: id\_rsa\_192.168.60.110和
id\_rsa\_192.168.60.110.pub;然后将 id\_rsa\_192.168.60.110.pub
文件的内容, 追加到sever的 \~/.ssh/authorized\_keys文件中,最后,
在本地用ssh命令的 -i 参数指定本地密钥, 并登录:  
\# ssh -i /root/.ssh/id\_rsa\_192.168.60.110 someone@192.168.60.110

scp也是一样的  
\# scp -i /root/.ssh/id\_rsa\_192.168.60.110 filename
someone@192.168.60.110:/home/someone

在文件.bashrc中加下两行，每次做同样的操作就不用敲入这样长的命令了：  
alias sshcell='ssh -i /root/.ssh/id\_rsa\_192.168.60.110
someone@192.168.60.110'

alias scpcell='scp -i /root/.ssh/id\_rsa\_192.168.60.110 filename
someone@192.168.60.110:/home/someone'

这样，直接键入一下指令实现ssh和scp自动登录：  
\# sshcell

\# scpcell

​3. 自动ssh/scp脚本  
如果需要从A，到B，然后才能够到C，那么需要ssh和scp两次，是比较麻烦的。

ssh自动登录：

\#!/usr/bin/expect -f  
set timeout 30

spawn ssh weiqiong@B

expect “password:”

send “pppppp/r”

expect “]\*”

send “ssh weiqiong@C/r”

expect “password:”

send “ppppp

<div class="posttagsblock">
</div>

