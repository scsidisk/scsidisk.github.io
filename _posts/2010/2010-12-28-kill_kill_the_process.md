---
layout: post
title: kill：杀掉进程
date: 2010-12-28 13:52
author: scsidisk
category: CentOS
tags: CentOS, Linux, Shell, 命令
Slug: kill_kill_the_process
---

作用：kill命令终止一个进程。

用法：kill [-s signal |-p] [-a]pid…

或 kill -l [ signal ]

主要选项如下

-s：指定发送的信号。

-p：模拟发送信号。

-l：指定信号的名称列表。

pid：要终止的进程的ID号。

signal：表示信号。

说明：kill可将指定的信息送至程序。预设的信息为SIGTERM（15），可将指定程序终止。若仍无法终止该程序，可使用SIGKILL（9）信息尝试强制删除程序。kill命令的工作原理是向Linux系统的内核发送一个系统操作信号和某个程序的进程标志号，然后系统内核就可以对进程标志号指定的进程进行操作。当需要中断一个前台进程的时候，通常使用Ctrl+C组合键；但是对于一个后台进程，就不是一个组合键所能解决的了，这时就必须使用kill命令。

应用实例如下。

（1）命令执行过程如果出错，用户可用"kill"来结束任务

对于在后台运行的进程，可以使用kill命令终止：

＃du -a / | sort -rn \> /tmp/du.sorted &

[1] 237

\#kill 237

或者使用命令：

＃du -a / | sort -rn \> /tmp/du.sorted &

[1] 237

\#kill %1

（2）对于僵尸进程，可以用kill-9来强制终止退出

比如一个程序已经彻底死掉，如果kill不加信号强度没有办法退出，最好的办法就是加信号强度-9，后面要接杀父进程。

比如：

\# ps aux |grep gaim

beinan 5031 9.0 2.3 104996 17484 ?

S 13:23 0:01 gaim

root 5036 0.0 0.0 5160 724 pts/3

S+ 13:24 0:00 grep gaim

kill命令族成员简介如下。

终止一个进程或终止一个正在运行的程序，一般通过kill，killall，pkill，xkill等进行。比如一个程序已经死掉，但又不能退出，这时就应该考虑应用这些工具。killall通过程序的名字，直接杀死所有进程。pkill和killall的应用方法差不多，也是直接杀死运行中的程序。如果想杀掉单个进程，可用kill来杀掉。xkill是在桌面用的杀死图形界面的程序。比如当Firefox出现崩溃不能退出时，单击鼠标就能杀死Firefox。当xkill运行时光标处会出来个人脑骨的图标，哪个图形程序崩溃了，一点就OK了。如果想终止xkill，就单击鼠标右键取消。

<div class="posttagsblock">
</div>

