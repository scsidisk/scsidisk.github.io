---
layout: post
title: 桥接模式构建 docker 网络
date: 2014-07-23 07:31
author: 智深
category: Virtualization
tags: 虚拟化 LXC Docker bridge
---


	# 主机环境 ubuntu server 14.04，虚拟机
	# eth1：192.168.56.101
	# eth2: 192.168.58.101
	# 192.168.56.0/24     管理网络
	# 192.168.58.0/24     数据网络，容器使用的网络

	# 切换到 root 用户执行以下命令


1、配置 Linux Bridge

     brctl addbr br100
     ip addr add 192.168.58.110/24 dev br100     # 给桥设置一个 IP，这样主机可以直接访问容器
     ip link set dev br100 up

2、配置 Docker 守护进程

     echo 'DOCKER_OPTS="-b=br100"' >> /etc/default/docker
     sudo service docker start

3、启动容器，设置网络模式为 none，自己配置容器网络

     sudo docker run -it --net=none ubuntu:14.04 /bin/bash

4、查看容器进程 id

     docker inspect -f '{{.State.Pid}}’ CONTAINER_ID
     pid=xxx

5、创建 namespaces 的目录

     mkdir -p /var/run/netns
     ln -s /proc/$pid/ns/net /var/run/netns/$pid

6、创建 veth 设备，分配给容器，绑定到桥

     ip link add vetha type veth peer name vethb
     brctl addif br100 vethb 
     ip link set vethb up
     ip link set vetha netns $pid
     ip netns exec $pid ip link set dev vetha name eth0 
     ip netns exec $pid ip link set eth0 up 
     ip netns exec $pid ip addr add 192.168.58.121/24 dev eth0 
     ip netns exec $pid ip route add default via 192.168.58.110

7、绑定 eth2 到 桥

     brctl addif br100 eth2
     ip addr del 192.168.58.101/24 dev eth2     # 删除 eth2 的 IP
     ip addr add 192.168.58.101/24 dev br100    # 把 eth2 的 IP 加到 桥中

Complete！！！ 

转自：http://my.oschina.net/astute/blog/293944