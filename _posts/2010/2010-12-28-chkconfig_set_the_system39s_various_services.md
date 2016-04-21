---
layout: post
title: chkconfig：设置系统的各种服务
date: 2010-12-28 13:53
author: scsidisk
category: Linux
tags: [CentOS, Shell, 命令]
---

作用：chkconfig命令检查、设置系统的各种服务。

用法：chkconfig [—add][—del][—list][系统服务]

或 chkconfig [—level <等级代号>][系统服务][on/off/reset]

主要选项如下。

—add：增加所指定的系统服务，让chkconfig指令得以管理它，并同时在系统启动的叙述文件内增加相关数据。

—del：删除所指定的系统服务，不再由chkconfig指令管理，并同时在系统启动的叙述文件内删除相关数据。

—level<等级代号>：指定读系统服务要在哪一个执行等级中开启或关闭。

说明：chkconfig提供了一个简单的命令行工具用于维护/etc/rc[0-6].d的路径层次，可以帮助系统管理员在这些路径中直接操作符号行，chkconfig的执行是通过chkconfig命令激发的，此命令目前在irix操作系统中存在，甚至包括了维护/etc/rc[0-6].d层次之外的设置信息。chkconfig有5个不同的函数：为管理器添加新服务，从管理器中移出服务，列出当前启动的服务信息，改变服务启动信息，检查特殊服务的启动状态。这是Red
Hat公司遵循GPL规则所开发的程序，它可查询操作系统在每一个执行等级中会执行哪些系统服务，其中包括各类常驻服务。

应用实例如下。

（1）显示所有服务的状态

# chkconfig —list

NetworkManager 0:off 1:off 2:off 3:off 4:off 5:off 6:off

acpid 0:off 1:off 2:on 3:on 4:on 5:on 6:off

anacron 0:off 1:off 2:on 3:on 4:on 5:on 6:off

apmd 0:off 1:off 2:on 3:on 4:on 5:on 6:off

atd 0:off 1:off 2:off 3:on 4:on 5:on 6:off

auditd 0:off 1:off 2:on 3:on 4:on 5:on 6:off

autofs 0:off 1:off 2:off 3:on 4:on 5:on 6:off

avahi-daemon 0:off 1:off 2:off 3:on 4:on 5:on 6:off

avahi-dnsconfd 0:off 1:off 2:off 3:off 4:off 5:off 6:off

bluetooth 0:off 1:off 2:on 3:on 4:on 5:on 6:off

capi 0:off 1:off 2:off 3:off 4:off 5:off 6:off

conman 0:off 1:off 2:off 3:off 4:off 5:off 6:off

cpuspeed 0:off 1:on 2:on 3:on 4:on 5:on 6:off

crond 0:off 1:off 2:on 3:on 4:on 5:on 6:off

cups 0:off 1:off 2:on 3:on 4:on 5:on 6:off

dnsmasq 0:off 1:off 2:off 3:off 4:off 5:off 6:off

dund 0:off 1:off 2:off 3:off 4:off 5:off 6:off

firstboot 0:off 1:off 2:off 3:on 4:off 5:on 6:off

gpm 0:off 1:off 2:on 3:on 4:on 5:on 6:off

haldaemon 0:off 1:off 2:off 3:on 4:on 5:on 6:off

hidd 0:off 1:off 2:on 3:on 4:on 5:on 6:off

ibmasm 0:off 1:off 2:off 3:off 4:off 5:off 6:off

ip6tables 0:off 1:off 2:on 3:on 4:on 5:on 6:off

iptables 0:off 1:off 2:on 3:on 4:on 5:on 6:off

irda 0:off 1:off 2:off 3:off 4:off 5:off 6:off

irqbalance 0:off 1:off 2:on 3:on 4:on 5:on 6:off

isdn 0:off 1:off 2:on 3:on 4:on 5:on 6:off

kudzu 0:off 1:off 2:off 3:on 4:on 5:on 6:off

lvm2-monitor 0:off 1:on 2:on 3:on 4:on 5:on 6:off

mcstrans 0:off 1:off 2:on 3:on 4:on 5:on 6:off

mdmonitor 0:off 1:off 2:on 3:on 4:on 5:on 6:off

mdmpd 0:off 1:off 2:off 3:off 4:off 5:off 6:off

messagebus 0:off 1:off 2:off 3:on 4:on 5:on 6:off

multipathd 0:off 1:off 2:off 3:off 4:off 5:off 6:off

netconsole 0:off 1:off 2:off 3:off 4:off 5:off 6:off

netfs 0:off 1:off 2:off 3:on 4:on 5:on 6:off

netplugd 0:off 1:off 2:off 3:off 4:off 5:off 6:off

network 0:off 1:off 2:on 3:on 4:on 5:on 6:off

nfs 0:off 1:off 2:off 3:off 4:off 5:off 6:off

nfslock 0:off 1:off 2:off 3:on 4:on 5:on 6:off

nscd 0:off 1:off 2:off 3:off 4:off 5:off 6:off

oddjobd 0:off 1:off 2:off 3:off 4:off 5:off 6:off

pand 0:off 1:off 2:off 3:off 4:off 5:off 6:off

pcscd 0:off 1:off 2:on 3:on 4:on 5:on 6:off

portmap 0:off 1:off 2:off 3:on 4:on 5:on 6:off

psacct 0:off 1:off 2:off 3:off 4:off 5:off 6:off

rawdevices 0:off 1:off 2:off 3:on 4:on 5:o

