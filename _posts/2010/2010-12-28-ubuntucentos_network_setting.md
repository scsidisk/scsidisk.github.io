---
layout: post
title: ubuntu/centos 上的双网卡设置
date: 2010-12-28 14:56
author: scsidisk
category: Linux
tags: [CentOS, Linux]
Slug: 2010-12-28-ubuntucentos_network_setting
---

我们经常有这样的需求，
服务器托管在机房，分配了一个外网IP，还想弄一个内网IP。怎么实现呢，
首先你需要有2块网卡(一般服务器主板都有2个集成网卡)。

第一步， 查看一下2块网卡是否已经识别， 命令 ： lspci | grep ‘Ethernet’
或者在 lspci里面找。如果找到 2 个Ethernet Controller 就说明没问题了。

设置外网IP 给连接外网的网口， 假设是 eth0,

那么在ubuntu中 ， vim /etc/network/interfaces ， 增加类似如下的语句，

auto eth0

iface eth0 inet static

address 222.73.44.222

netmask 255.255.255.192

gateway 222.73.44.193

在 centos里面 ， vim /etc/sysconfig/network-scripts/ifcfg-eth0 ， 形如

DEVICE=eth0

BOOTPROTO=none

HWADDR=00:15:17:9d:0f:51

ONBOOT=yes

NETMASK=255.255.255.128

IPADDR=61.129.52.159

GATEWAY=61.129.52.254

TYPE=Ethernet

设置内网IP，
<span style="color: #ff0000;">注意不能设置网关</span>，否则会出问题，
只要 设置 IP 和 netmask就可以了，

在ubuntu中 ， vim /etc/network/interfaces ， 增加类似如下的语句，

auto eth1

iface eth1 inet static

address 192.168.0.222

netmask 255.255.255.0

在 centos里面 ， vim /etc/sysconfig/network-scripts/ifcfg-eth1 ， 形如

DEVICE=eth1

BOOTPROTO=none

HWADDR=00:15:17:9d:0f:51

ONBOOT=yes

NETMASK=255.255.255.0

IPADDR=192.168.0.159

TYPE=Ethernet

然后重启网络就可以了,

(ubuntu) sudo /etc/init.d/networking restart ,

(centos) /etc/init.d/network restart .

<div class="posttagsblock">
</div>

