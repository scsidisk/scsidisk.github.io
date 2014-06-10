---
layout: post
title: 在nginx中配置ip直接访问的默认站点
date: 2013-01-28
author: scsidisk
category: Nginx
tags: MacOSX, Nginx
---

在nginx中，每个站点都是由一个server段定义的，这里面设定了监听的ip和端口，站点的域名，根目录等信息。但一般来说vps主机上每个ip上会对应几个不同的站点。这样就会出现一个问题，直接访问这个ip的话，访问的会是哪个站点？

答案是这样的：在Listen ip:port; 这个指令行中，有一个参数default，指定了它后，这个server段就会是这个ip的默认站点；如果没有这个参数，那么默认ip直接访问的是nginx.conf中出现的第一个server段对应的站点。

```nginx
server { 
	listen: 127.0.0.1:80 default; 
	server_name shuai.be; 
	...; 
}
```