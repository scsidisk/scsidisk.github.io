---
layout: post
title: does not map back to the address
date: 2010-12-28 13:51
author: scsidisk
category: CentOS
tags: CentOS, Linux, Mac, Shell, SSH
Slug: does_not_map_back_to_the_address
---

```
[\~]\$ ssh 192.168.0.233

Address 192.168.0.233 maps to localhost, but this does not map back to
the address - POSSIBLE BREAK-IN ATTEMPT!

Last login: Thu Nov 26 10:29:29 2009 from 192.168.0.235
```

用 ssh 时，经常看到这样的警告信息，挺闹心的，因为DNS服务器把 192.168.0.x
的地址都反向解析成 localhost ，而DNS服务器不是咱们自己的，没办法改。

找到一种办法，编辑 ssh 客户端的 /etc/hosts 文件，把出问题的 IP
地址和主机名加进去，就不会报这样的错了。


