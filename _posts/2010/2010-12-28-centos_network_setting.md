---
layout: post
title: "centos网络配置"
date: 2010-12-28 15:22
author: scsidisk
categories: Linux
tags: CentOS, Linux, 网卡, 配置
Slug: centos%e7%bd%91%e7%bb%9c%e9%85%8d%e7%bd%ae
----

ip设置：

    /etc/sysconfig/network-scripts/ifcfg-eth0

配置说明：

    DEVICE=eth0
    BOOTPROTO=static
    HWADDR= MAC地址，这个一般都识别出来了，不用改
    IPADDR= IP地址
    NETMASK= 子网掩码
    GATEWAY= 网关

dns配置

    /etc/resolv.conf

service network start|stop|restart..


