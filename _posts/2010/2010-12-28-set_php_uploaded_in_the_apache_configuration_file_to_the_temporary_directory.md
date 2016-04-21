---
layout: post
title: 在apache配置文件中设置php上传临时目录
date: 2010-12-28 14:57
author: scsidisk
category: Linux
tags: [Apache, PHP, upload]
Slug: set_php_uploaded_in_the_apache_configuration_file_to_the_temporary_directory
---

在服务器上配置webmail(比如我最喜欢的SquirrelMail)时，出于服务器安全考虑，一般在apache配置文件中作

php\_admin\_value open\_basedir \<path to web root\>

的限制，防止php程序浏览整个硬盘，这个限制在使用虚拟主机的服务器上使用的更多。

然而这个安全措施带来一个隐含的限制，就是php的上传临时目录(默认为/tmp)无法被php程序访问，导致webmail中上传附件时失败，比如SquirrelMail提示“Could
not move/copy file. File Not
Attached.”(“无法移动/复制文件。文件需要被附在邮件上”)。

通过在apache配置文件中添加一个设置

php\_admin\_value upload\_tmp\_dir \<path to temp dir\>

让php程序在上传时使用指定的目录作为临时文件目录。

当然，要注意此目录的权限设置要让apache的运行用户能写入。

实例：

php\_admin\_value open\_basedir /www/mail.yourdomaim.com

php\_admin\_value upload\_tmp\_dir /www/mail.yourdomaim.com/temp

当然，从来不对php作限制的人是不会遇到这个问题的(只会遇到服务器被黑)。

小小经验，偶为此郁闷了很久。

来源：http://www.loveunix.net/thread-9915-1-1.html
