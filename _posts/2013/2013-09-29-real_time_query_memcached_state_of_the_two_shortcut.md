---
layout: post
title: "实时查询memcached状态的两个快捷命令"
date: 2013-09-29 17:25
author: scsidisk
categories: PHP
tags: Linux, memcache, PHP
---

查询实时的状态，类似于“top”命令。

在命令行下执行任一命令（第二个办法需要通过php）：

1. 使用shell

```
​$ watch "echo stats | nc 127.0.0.1 11211"
```

结果如下：

```
STAT pid 13785
STAT uptime 1377436
STAT time 1227764242
STAT version 1.2.6
STAT pointer\_size 32
STAT rusage\_user 9.265591
STAT rusage\_system 36.541444
STAT curr\_items 6
STAT total\_items 1093
STAT bytes 24628
STAT curr\_connections 19
STAT total\_connections 79352
STAT connection\_structures 117
STAT cmd\_get 609575
STAT cmd\_set 1093
STAT get\_hits 608483
STAT get\_misses 1092
STAT evictions 0
STAT bytes\_read 14703639
STAT bytes\_written 2297950214
STAT limit\_maxbytes 52428800
STAT threads 1
END
```

2. 使用php进行查询

```
​watch '/www/php/bin/php -r '"'"'\$m=new Memcache;\$m-\>connect("127.0.0.1", 11211);print\_r(\$m-\>getstats());'"'"
```

结果如下：

```
Array
(
[pid] =\> 13785
[uptime] =\> 1377469
[time] =\> 1227764275
[version] =\> 1.2.6
[pointer\_size] =\> 32
[rusage\_user] =\> 9.266591
[rusage\_system] =\> 36.541444
[curr\_items] =\> 6
[total\_items] =\> 1093
[bytes] =\> 24628
[curr\_connections] =\> 16
[total\_connections] =\> 79372
[connection\_structures] =\> 117
[cmd\_get] =\> 609593
[cmd\_set] =\> 1093
[get\_hits] =\> 608501
[get\_misses] =\> 1092
[evictions] =\> 0
[bytes\_read] =\> 14704073
[bytes\_written] =\> 2298032330
[limit\_maxbytes] =\> 52428800
[threads] =\> 1
)
```

上面命令假设memcached按默认情况运行，即服务器地址为127.0.0.1，端口为11211。请按实际情况修改。

转载:http://blog.csdn.net/haohappy2004/article/details/3390336
