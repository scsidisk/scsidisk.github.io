---
layout: post
title: libmemcache
date: 2012-07-01 12:55:55
author: scsidisk
category: Linux
tags: [Memcache]
---

libmemcache
===========

libmemcache 是 memcached C的客户端之一。我准备使用这个玩意。
官网：http://people.freebsd.org/~seanc/libmemcache/
下载：http://people.freebsd.org/~seanc/libmemcache/libmemcache-1.4.0.rc2.tar.bz2

编译：

```
./configure
make
make install
```

写一个有libmemcache函数的程序

然后

```
gcc a.c -o a.cgi -L/usr/local/include -lmemcache
```

运行

```
./a.cgi
```

提示类似上面的“error while loading shared libraries”的错误，查了一下可用"ldconfig"解决这个问题，那上面的可否用这个命令解决呢？

上面的gcc编译的时候，带有参数“-lmemcache”。
-l参数后面跟的库名有规则的，库的命名方式有libxxxx.so或libxxxx.a，编译时要用-lxxxx就可以了。
而“-l”的意思就是代表是个“lib”了。

所以在你自己编写动态库或者静态库时，命名还是得按照 libxxxx.so的形式来。

不得不认为libmemcache有点糟糕，编译它自带的一个程序编译竟然有问题，很无语！

```
[root@login benchmark]# gcc benchmark.c -L/usr/local/include -lmemcache
benchmark.c: In function `main':
benchmark.c:100: warning: passing arg 2 of `mc_set' discards qualifiers from pointer target type
benchmark.c:108: warning: passing arg 2 of `mc_set' discards qualifiers from pointer target type
benchmark.c:121: warning: passing arg 2 of `mc_req_add' discards qualifiers from pointer target type
benchmark.c:145: warning: passing arg 2 of `mc_delete' discards qualifiers from pointer target type
benchmark.c:153: warning: passing arg 2 of `mc_add' discards qualifiers from pointer target type
benchmark.c:167: warning: passing arg 2 of `mc_delete' discards qualifiers from pointer target type
```

解决方法：

找到passing arg 2这个参数的定义：

```
const char *key;
```

把const去了：

```
char *key;
```

这样就好了！

然后：

```
[root@login memcache-test]# ./a.out
```

执行时可能报以下的错误：

```
./a.out: error while loading shared libraries: libmemcache.so.0: cannot open shared object file: No such file or directory
```

解决方法：

```
执行ldconfig命令即可！
```

附：

http://kapoc.blogdriver.com/kapoc/1200549.html

ldconfig命令 - - 动态链接库管理命令

为了让动态链接库为系统所共享,还需运行动态链接库的管理命令--ldconfig.此执行程序存放在/sbin目录下.

ldconfig命令的用途,主要是在默认搜寻目录(/lib和/usr/lib)以及动态库配置文件/etc/ld.so.conf内所列的目录下,搜索出可共享的动态链接库(格式如前介绍,lib*.so*),进而创建出动态装入程序(ld.so)所需的连接和缓存文件.缓存文件默认为/etc/ld.so.cache,此文件保存已排好序的动态链接库名字列表.

ldconfig通常在系统启动时运行,而当用户安装了一个新的动态链接库时,就需要手工运行这个命令.