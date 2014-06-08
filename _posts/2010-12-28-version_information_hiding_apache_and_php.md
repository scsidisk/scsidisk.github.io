---
layout: post
title: 隐藏apache和php的版本信息
date: 2010-12-28 14:56
author: scsidisk
category: Apache
tags: Apache
---

web server避免一些不必要的麻烦，可以把apache和php的版本信息不显示

隐藏 Apache 版本信息  
/etc/apache2/apache2.conf 或 /etc/httpd/conf/httpd.conf

ServerTokens ProductOnly  
ServerSignature Off

重启 apache  
现在 http 头里面只看到:  
Server: Apache

隐藏 PHP 版本  
php.ini

expose\_php On  
改成  
expose\_php Off

重启apache后，php版本在http头中隐藏了。

详解 ：

为了防止某些别有用心的家伙窥视我们的服务器，应该做些什么.  

我们来看一下相关的2个参数，分别为ServerTokens和ServerSignature,通过控制这2个阀门应该就能起到一些作用，比如我们可以在配置文件中这么写：  
ServerTokens Prod  
ServerSignature Off

ServerTokens  

用于控制服务器是否相应来自客户端的请求，向客户端输出服务器系统类型或内置模块等重要的系统信息。在主配置文件中提供全局控制默认阀值为”Full”(ServerTokens
Full），所以，如果你的Linux发行版本没有更改过这个阀值的话，所有与你的系统有关的敏感信息都会向全世界公开。比如RHEL会将该阀值更改为”ServerTokens
OS”，而Debian依然使用默认的”Full”阀值  
以apache-2.0.55为例，阀值可以设定为以下某项（后面为相对应的Banner
Header)：  
Prod \>\>\> Server: Apache  
Major \>\>\> Server: Apache/2  
Minor \>\>\> Server: Apache/2.0  
Minimal \>\>\> Server: Apache/2.0.55  
OS \>\>\> Server: Apache/2.0.55 (Debian)  
Full (or not specified) default \>\>\> Server: Apache/2.0.55 (Debian)
PHP/5.1.2-1+b1 mod\_ssl/2.0.55 OpenSSL/0.9.8b

ServerSignature  
控制由系统生成的页面（错误信息，mod\_proxy ftp directory
listing等等）的页脚中如何显示信息。

可在全局设置文件中控制，或是通过.htaccess文件控制  
默认为”off”(ServerSignature
Off),有些Linux发行版本可能会打开这个阀门，比如Debian在默认的虚拟主机上默认将这个阀门设置为开放  

全局阀门的阀值会被虚拟主机或目录单位的配置文件中的阀值所覆盖，所以，必须确保这样的事情不应该发生  
可用的阀值为下面所示：  
Off (default): 不输出任何页脚信息
（如同Apache1.2以及更旧版本，用于迷惑）  
On:输出一行关于版本号以及处于运行中的虚拟主机的ServerName
(2.0.44之后的版本，由ServerTokens负责是否输出版本号）  
EMail: 创建一个发送给ServerAdmin的”mailto”

注意＊上述关于如何设置2个阀门从而尽量减少敏感信息泄露的方法，并不会使你的服务器真的更安全，如果你现在使用的版本比较旧，请务必尽快将系统升级，降低被蠕虫攻击的风险。
