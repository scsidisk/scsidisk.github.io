---
layout: post
title: sudo的使用说明
date: 2010-12-28 14:55
author: scsidisk
category: Linux
tags: [sudo]
Slug: instructions_for_use_of_sudo
---

“sudo”是Unix/Linux平台上的一个非常有用的工具，它允许系统管理员分配给普通用户一些合理的“权利”，让他们执行一些只有超级用户或其他特许用户才能完成的任务，比如：运行一些像mount，halt，su之类的命令，或者编辑一些系统配置文件，像/etc/mtab，/etc/samba/smb.conf等。这样以来，就不仅减少了root用户的登陆次数和管理时间，也提高了系统安全性。

一. sudo的特点

sudo扮演的角色注定了它要在安全方面格外谨慎，否则就会导致非法用户攫取root权限。同时，它还要兼顾易用性，让系统管理员能够更有效，更方便地使用它。sudo设计者的宗旨是：给用户尽可能少的权限但仍允许完成他们的工作。所以，sudo

有以下特点：

\# 1. sudo能够限制指定用户在指定主机上运行某些命令。

\# 2.
sudo可以提供日志，忠实地记录每个用户使用sudo做了些什么，并且能将日志传到中心主机或者日志服务器。

\# 3.
sudo为系统管理员提供配置文件，允许系统管理员集中地管理用户的使用权限和使用的主机。它默认的存放位置是/etc/sudoers。

\# 4.

sudo使用时间戳文件来完成类似“检票”的系统。当用户执行sudo并且输入密码后，用户获得了一张默认存活期为5分钟的“入场券”（默认值可以在编译的时候改变）。超时以后，用户必须重新输入密码。

二. sudo命令

sudo程序本身就是一个设置了SUID位的二进制文件。我们可以检查一下它的权限：

\$ls -l /usr/bin/sudo

---s--x--x 2 root root 106832 02-12 17:41 /usr/bin/sudo

它的所有者是root，所以每个用户都可以像root那样执行该程序。设置了SUID的程序在运行时可以给使用者以所有者的EUID。这也是为什么设置了SUID的程序必须小心编写。但是设置一个命令文件的SUID和用sudo来运行它是不同的概念，它们起着不同的作用。

sudo的配置都记录在/etc/sudoers文件中，我们下面将会详细说明。配置文件指明哪些用户可以执行哪些命令。要使用sudo，用户必须提供一个指定用户名和密码。注意：sudo需要的不是目标用户的密码，而是执行sudo的用户的密码。如果不在sudoers中的用户通过sudo执行命令，sudo会向管理员报告这一事件。用户可以通过sudo
-v来查看自己是否是在sudoers
之中。如果是，它还可以更新你的“入场券”上的时间；如果不是，它会提示你，但不会通知管理员。

sudo命令格式如下：

sudo -K | -L | -V | -h | -k | -l | -vsudo [-HPSb] [-a auth\_type] [-c

class|-] [-p prompt] [-u username|\#uid] {-e file [...] | -i | -s |
command}

下面我们再来看一下sudo其它常用的一些参数：

选项 含义 作用

sudo -h Help 列出使用方法，退出。

sudo -V Version 显示版本信息，并退出。

sudo -l List
列出当前用户可以执行的命令。只有在sudoers里的用户才能使用该选项。

sudo -u username|\#uid User
以指定用户的身份执行命令。后面的用户是除root以外的，可以是用户名，也可以是\#uid。

sudo -k Kill 清除“入场卷”上的时间，下次再使用sudo时要再输入密码。

sudo -K Sure kill
与-k类似，但是它还要撕毁“入场卷”，也就是删除时间戳文件。

sudo -b command Background 在后台执行指定的命令。

sudo -p prompt command Prompt
可以更改询问密码的提示语，其中%u会代换为使用者帐号名称，%h会显示主机名称。非常人性化的设计。

sudo -e file Edit 不是执行命令，而是修改文件，相当于命令sudoedit。

还有一些不常用的参数，在手册页sudo(8)中可以找到。

三. 配置sudo

配置sudo必须通过编辑/etc/sudoers文件，而且只有超级用户才可以修改它，还必须使用visudo编辑。之所以使用visudo有两个原因，一是它能够防止

两个用户同时修改它；二是它也能进行有限的语法检查。所以，即使只有你一个超级用户，你也最好用visudo来检查一下语法。

visudo默认的是在vi里打开配置文件，用vi来修改文件。我们可以在编译时修改这个默认项。visudo不会擅自保存带有语法错误的配置文件，它会提示你出现的问题

，并询问该如何处理，就像：

\>\>\> sudoers file: syntax error, line 22 \<\<\<What now? e

此时我们有三种选择：键入“e”是重新编辑，键入“x”是不保存退出，键入“Q”是退出并保存。如果真选择Q，那么sudo将不会再运行，直到错误被纠正。

现在，我们一起来看一下神秘的配置文件，学一下如何编写它。让我们从一个简单的例子开始：让用户foobar可以通过sudo执行所有root可执行的命令。以root身份用visudo打开配置文件，可以看到类似下面几行：

\# Runas alias specification

\# User privilege specificationroot ALL=(ALL)ALL

我们一看就明白个差不多了，root有所有权限，只要仿照现有root的例子就行，我们在下面加一行（最好用tab作为空白）：

foobar ALL=(ALL) ALL

保存退出后，切换到foobar用户，我们用它的身份执行命令：

[foobar@localhost \~]\$ ls /root

ls: /root: 权限不够

[foobar@localhost \~]\$ sudo ls /root

Password:

anaconda-ks.cfg Desktop install.log install.log.syslog

好了，我们限制一下foobar的权利，不让他为所欲为。比如我们只想让他像root那样使用ls和ifconfig，把那一行改为：

foobar localhost= /sbin/ifconfig, /bin/ls

再来执行命令：

[foobar@localhost \~]\$ sudo head -5 /etc/shadow

Password:

Sorry, user foobar is not allowed to execute '/usr/bin/head -5
/etc/shadow' as root on localhost.localdomain.

[foobar@localhost \~]\$ sudo /sbin/ifconfigeth0 Linkencap:Ethernet
HWaddr 00:14:85:EC:E9:9B...

现在让我们来看一下那三个ALL到底是什么意思。第一个ALL是指网络中的主机，我们后面把它改成了主机名，它指明

foobar可以在此主机上执行后面的命令。第二个括号里的ALL是指目标用户，也就是以谁的身份去执行命令。最后一个

ALL当然就是指命令名了。例如，我们想让foobar用户在linux主机上以jimmy或rene的身份执行kill命令，这样编写配置文件：

foobar linux=(jimmy,rene) /bin/kill

但这还有个问题，foobar到底以jimmy还是rene的身份执行？这时我们应该想到了sudo
-u了，它正是用在这种时候。 foobar可以使用sudo -u jimmy kill PID或者sudo
-u rene kill
PID，但这样挺麻烦，其实我们可以不必每次加-u，把rene或jimmy设为默认的目标用户即可。再在上面加一行：

Defaults:foobar runas\_default=rene

Defaults后面如果有冒号，是对后面用户的默认，如果没有，则是对所有用户的默认。就像配置文件中自带的一行：

Defaults env\_reset

另一个问题是，很多时候，我们本来就登录了，每次使用sudo还要输入密码就显得烦琐了。我们可不可以不再输入密码呢？当然可以，我们这样修改配置文件：

foobar localhost=NOPASSWD: /bin/cat, /bin/ls

再来sudo一下：

[foobar@localhost \~]\$ sudo ls /rootanaconda-ks.cfg Desktop install.log

install.log.syslog

当然，你也可以说“某些命令用户foobar不可以运行”，通过使用!操作符，但这不是一个好主意。因为，用!操作符来从ALL中“剔出”一些命令一般是没什么效果的，一个用户完全可以把那个命令拷贝到别的地方，换一个名字后再来运行。

四. 日志与安全

sudo为安全考虑得很周到，不仅可以记录日志，还能在有必要时向系统管理员报告。但是，sudo的日志功能不是自动的，必须由管理员开启。这样来做：

\# touch /var/log/sudo

\# vi /etc/syslog.conf

在syslog.conf最后面加一行（必须用tab分割开）并保存：

local2.debug /var/log/sudo

重启日志守候进程，

ps aux | grep syslogd

把得到的syslogd进程的PID（输出的第二列是PID）填入下面：

kill –HUP PID

这样，sudo就可以写日志了：

[foobar@localhost \~]\$ sudo ls /rootanaconda-ks.cfg

Desktop install.log

install.log.syslog

\$cat /var/log/sudoJul 28 22:52:54 localhost sudo: foobar :

TTY=pts/1 ; PWD=/home/foobar ; USER=root ; COMMAND=/bin/ls /root

不过，有一个小小的“缺陷”，sudo记录日志并不是很忠实：

[foobar@localhost \~]\$ sudo cat /etc/shadow \> /dev/null

[foobar@localhost \~]\$

cat /var/log/sudo...Jul 28 23:10:24 localhost sudo: foobar : TTY=pts/1 ;

PWD=/home/foobar ; USER=root ; COMMAND=/bin/cat /etc/shadow

重定向没有被记录在案！为什么？因为在命令运行之前，shell把重定向的工作做完了，sudo根本就没看到重定向。这也有个好处，下面的手段不会得逞：

[foobar@localhost \~]\$ sudo ls /root \> /etc/shadowbash: /etc/shadow:
权限不够

sudo 有自己的方式来保护安全。以root的身份执行sudo

-V，查看一下sudo的设置。因为考虑到安全问题，一部分环境变量并没有传递给sudo后面的命令，或者被检查后再传递的，比如：PATH，HOME，

SHELL等。当然，你也可以通过sudoers来配置这些环境变量。

如上所见，sudo对于控制和审查root的访问很有帮助，它能让系统管理员更有效，安全地管理系统。掌握sudo的正确使用也是对于系统管理员的良好训练。本文只是初步地介绍了sudo
的使用，了解更多请参考sudoers(5)和sudo(8)手册页。

<div class="posttagsblock">
</div>

