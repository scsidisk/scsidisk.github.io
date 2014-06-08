---
layout: post
Title: CentOS安装中文支持
Date: 2010-12-28 13:51
Author: scsidisk
Category: CentOS
Tags: CentOS, Linux
Slug: centos_install_chinese_support
---

CentOS安装中文支持

Centos默认安装如果是英文的话,选择中文不正常，firefox也只能见到方块的字.但有一个方法，其实很容易解决安装这二个包,然后你就可以见到中文啦.为了这个问题，我可是研究了不少时间哦。老是不记的包的名字

有时可能会用到哦，象我喜欢最小化安装，然后在安装别的软件时间，就非常有用.

```
#rpm -ivh fonts-chinese-3.02-12.el5.noarch.rpm
#rpm -ivh fonts-ISO8859-2-75dpi-1.0-17.1.noarch.rpm
或者
#yum install fonts-chinese-3.02-12.el5.noarch.rpm
#yum install fonts-ISO8859-2-75dpi-1.0-17.1.noarch.rpm
```

使用PUTTY连入后把编码改成UTF-8，centos编码为：en_US.UTF-8



