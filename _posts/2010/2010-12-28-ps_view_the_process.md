---
layout: post
title: "ps：查看进程"
date: 2010-12-28 13:53
author: scsidisk
categories: Linux
tags: CentOS, Linux, Shell, 命令
---

作用：ps命令主要用于查看系统中进程的状态。

用法：ps [选项]

主要选项如下。

-a：显示系统中所有进程的信息。

-e：显示所有进程的信息。

-f：显示进程的所有信息。

-l：以长用法显示进程信息。

-r：只显示正在运行的进程。

-u：显示面向用户的用法（包括用户名、CPU及内存使用情况等信息）。

-x：显示所有非控制终端上的进程信息。

-p：显示由进程ID指定的进程的信息。

-t：显示指定终端上的进程的信息。

说明：要对进程进行监测和控制，首先要了解当前进程的情况，也就是需要查看当前进程。ps命令就是最基本、也是非常强大的进程查看命令。根据显示的信息可以确定哪个进程正在运行、哪个进程被挂起、进程已运行了多久、进程正在使用的资源、进程的相对优先级，以及进程的标志号（PID）。所有这些信息对用户都很有用，对于系统管理员来说更为重要。使用"ps
aux"命令可以获得终端上所有用户的有关进程的所有信息，下面将结合图4-3讲解进程的基本信息。

![103747619](/static/images/2010/12/103747619-300x134.jpg)

（点击查看大图）图4-3 ps aux命令详解

图4-3的第二行代码中，USER表示启动进程用户。PID表示进程标志号。%CPU表示运行该进程占用CPU的时间与该进程总的运行时间的比例。%MEM表示该进程占用内存和总内存的比例。VSZ表示占用的虚拟内存大小，以KB为单位。RSS为进程占用的物理内存值，以KB为单位。TTY表示该进程建立时所对应的终端，"?"表示该进程不占用终端。STAT表示进程的运行状态，包括以下几种代码：D，不可中断的睡眠；R，就绪（在可运行队列中）；S，睡眠；T，被跟踪或停止；Z，终止（僵死）的进程；Z，不存在，但暂时无法消除；W，没有足够的内存分页可分配；\<，高优先序的进程；N，低优先序的进程；L，有内存分页分配并锁在内存体内（实时系统或I/O）。START为进程开始时间。TIME为执行的时间。COMMAND是对应的命令名。

应用实例如下。

（1）按内存占用情况对进程排序

\# ps auxw --sort=rss

（2）在进行系统维护时，如果CPU负载突然增加，而又不知道是哪一个进程造成的

＃ps auxw --sort=%cpu

<div class="posttagsblock">
</div>

