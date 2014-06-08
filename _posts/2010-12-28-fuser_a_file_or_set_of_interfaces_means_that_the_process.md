---
layout: post
Title: fuser：用文件或者套接口表示进程
Date: 2010-12-28 14:54
Author: scsidisk
Category: CentOS
Tags: CentOS, Shell, 命令
Slug: fuser_a_file_or_set_of_interfaces_means_that_the_process
---

作用：fuser命令用文件或者套接口表示进程。

用法：fuser [-a | -s | -c] [-4 | -6] [-n space] [-k [-i] [-signal]]
[-muvf] name …

或 fuser -l

或 fuser -V

主要选项如下。

-a：显示在命令行指定的所有文件，默认情况下，至少被一个进程访问的文件才能显示出来。

-c：同选项-m，用于同Posix进行兼容。

-f：忽略，用于同Posix进行兼容。

-i：结束进程前询问用户意见。

-k：结束正在访问文件的所有进程。

-l：列出所有已知的信号名字。

-m：挂载文件系统。

-n<space>：选择一个不同的名字空间，名字空间是指文件（默认为文件名）、udp和tcp。

-s：不显示处理信息，选项-u和-v在此模式下将被忽略，选项-a不能与该选项一起使用。

-signal：结束进程时使用指定的信号而不是SIGKILL，当不使用选项-k时，该选项将被忽略。

-u PID：显示用户名。

-v：显示运行时的详细信息。

-V：显示版本信息。

应用实例如下。

（1）列出所有已知的信号名字

# fuser -l

HUP INT QUIT ILL TRAP ABRT IOT BUS FPE KILL

USR1 SEGV USR2 PIPE ALRM TERM

STKFLT CHLD CONT STOP TSTP TTIN TTOU URG

XCPU XFSZ VTALRM PROF WINCH IO PWR SYS

UNUSED

（2）显示进程

例如，显示与/home/cjh/目录相关的所有进程，在命令行提示符下输入：

# fuser -a /home/cjh

/home/cjh: 19169c 19197c

（3）结束正在访问文件的所有进程

例如，结束正在访问目录/home/cjh/tmp/的所有进程，在命令行提示符下输入：

#fuser -k /home/cjh/tmp/

/home/cjh/tmp/: 19169c

（4）显示用户名

例如，显示所有访问目录/home/cjh/的进程，并显示进程的用户名，在命令行提示符下输入：

# fuser -u /home/cjh/

/home/cjh/: 19245c(cjh)

（5）列出使用/etc/passwd文件的本地进程的进程号

#fuser /etc/passwd

（6）列出正在使用已从给定文件系统删除的文件的全部进程

#fuser -d /usr


