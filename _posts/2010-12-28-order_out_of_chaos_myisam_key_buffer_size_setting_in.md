---
layout: post
title: 拨乱反正：MyISAM中key_buffer_size的设置
date: 2010-12-28 13:52
author: scsidisk
category: Database
tags: [MySQL]
Slug: order_out_of_chaos_myisam_key_buffer_size_setting_in
---

作者：老王

一直以来，多数人在使用MyISAM时都是按照增大Key\_read\_requests /
Key\_reads的原则来设置key\_buffer\_size的，没想到这竟然是错误的！这次给大家醍醐灌顶的仍然是MySQL
Performance Blog，详细描述参考：Why you should ignore MySQL’s key cache
hit ratio。

Key\_read\_requests和Key\_reads就是两个计数器，它们的含义如下：

Key\_read\_requests：从缓存读取索引的请求次数。

Key\_reads：从磁盘读取索引的请求次数。

通常人们认为Key\_read\_requests /
Key\_reads越大越好，否则就应该增大key\_buffer\_size的设置，但通过计数器的比例来调优有两个问题：

问题一：比例并不显示数量的绝对值大小

问题二：计数器并没有考虑时间因素

虽说Key\_read\_requests大比小好，但是对于系统调优而言，更有意义的应该是单位时间内的Key\_reads：

Key\_reads / Uptime

你可以通过命令行得到一个实时的数据结果，比如：

\# mysqladmin ext -ri10 | grep Key\_reads

| Key\_reads | 83777189 |

| Key\_reads | 211 |

| Key\_reads | 177 |

| Key\_reads | 202 |

提示：命令里的mysqladmin ext其实就是mysqladmin
extended-status，你甚至可以简写成mysqladmin e。

其中第一行表示的是汇总数值，所以这里不必考虑，下面的每行数值都表示10秒内的数据变化，从这份数据可以看出每10秒系统大约会出现200次Key\_reads访问，折合到每1秒就是20次左右，至于这个数值到底合理与否，就由服务器的磁盘能力而定了。

顺便说一句，为啥数据按10秒取样，而不是直接按1秒取样？这里看看按1秒的结果：

\# mysqladmin ext -ri1 | grep Key\_reads

| Key\_reads | 83776743 |

| Key\_reads | 7 |

| Key\_reads | 7 |

| Key\_reads | 38 |

可以看到，由于时间段过小，数据变化比较剧烈，不容易直观估计大小，所以通常数据按照10秒或者60秒之类的时间段来取样是更好的。

忘记：Key\_read\_requests / Key\_reads

牢记：Key\_reads / Uptime

来源：http://hi.baidu.com/thinkinginlamp/blog/item/53592838e60b3e2fb8998ffd.html

 
