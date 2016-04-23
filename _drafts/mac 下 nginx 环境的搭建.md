---
title: "mac 下 nginx 环境的搭建"
tags: []
notebook: Nginx
---

nginx 是一个轻量级的高性能HTTP 以及反向代理服务器，今天在MAC 上安装成功。


我是通过brewhome 来安装的，很简单。brew install nginx 一路顺畅。。。

下面是安装信息。

$ brew search nginx

$ brew install nginx

启动nginx ，sudo nginx ;访问localhost:8080 发现已出现nginx的欢迎页面了。

备注： ln -s /usr/local/sbin/nginx /usr/bin/nginx 做了个软连接。

常用的指令有：

nginx -V 查看版本，以及配置文件地址

nginx -v 查看版本

nginx -c filename 指定配置文件

nginx -h 帮助

nginx -s [reload\reopen\stop\quit]

brewhome 常用的指令：

brew search mysql : 搜索具体的程序包

brew install mysql : 安装具体的程序包

brew info mysql : 查看具体程序的信息

brew uninstall mysql : 卸载具体的应用（这里只是用mysql 作个例子）

我这里的配置文件地址：/usr/local/etc/nginx/nginx.conf

编辑内容，可以制定web 目录，以及PHP 、python 等。。
