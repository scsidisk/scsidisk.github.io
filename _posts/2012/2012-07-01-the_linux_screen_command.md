---
layout: post
title: "Linux screen 命令详解"
date: 2012-07-01
author: scsidisk
categories: Linux
tags: Linux
---

### 功能说明：

使用telnet或SSH远程登录linux时，如果连接非正常中断，重新连接时，系统将开一个新的session，无法恢复原来的session.screen命令可以解决这个问题。Screen工具是一个终端多路转接器，在本质上，这意味着你能够使用一个单一的终端窗口运行多终端的应用。

### 语法：

```
screen [-AmRvx -ls -wipe][-d <作业名称>][-h <行数>][-r <作业名称>][-s ][-S <作业名称>]
```

#### 补充说明：

screen为多重视窗管理程序。此处所谓的视窗，是指一个全屏幕的文字模式画面。通常只有在使用telnet登入主机或是使用老式的终端机时，才有可能用到screen程序。

#### 参数：

```
-A 　将所有的视窗都调整为目前终端机的大小。
-d <作业名称> 　将指定的screen作业离线。
-h <行数> 　指定视窗的缓冲区行数。
-m 　即使目前已在作业中的screen作业，仍强制建立新的screen作业。
-r <作业名称> 　恢复离线的screen作业。
-R 　先试图恢复离线的作业。若找不到离线的作业，即建立新的screen作业。
-s 　指定建立新视窗时，所要执行的shell。
-S <作业名称> 　指定screen作业的名称。
-v 　显示版本信息。
-x 　恢复之前离线的screen作业。
-ls或--list 　显示目前所有的screen作业。
-wipe 　检查目前所有的screen作业，并删除已经无法使用的screen作业。
```

### 常用screen参数：

```
screen -S yourname -> 新建一个叫yourname的session
screen -ls -> 列出当前所有的session
screen -r yourname -> 回到yourname这个session
screen -d yourname -> 远程detach某个session
screen -d -r yourname -> 结束当前session并回到yourname这个session
````

在每个screen session 下，所有命令都以 `ctrl+a(C-a)` 开始。

```
C-a ? -> Help，显示简单说明
C-a c -> Create，开启新的 window
C-a n -> Next，切换到下个 window
C-a p -> Previous，前一个 window
C-a 0..9 -> 切换到第 0..9 个window
Ctrl+a [Space] -> 由視窗0循序換到視窗9
C-a C-a -> 在两个最近使用的 window 间切换
C-a x -> 锁住当前的 window，需用用户密码解锁
C-a d -> detach，暂时离开当前session，将目前的 screen session (可能含有多个 windows)
	丢到后台执行，并会回到还没进 screen 时的状态，此时在 screen session 里    每个 window
	内运行的 process (无论是前台/后台)都在继续执行，即使 logout 也不影响。
C-a z -> 把当前session放到后台执行，用 shell 的 fg 命令則可回去。
C-a w -> Windows，列出已开启的 windows 有那些
C-a t -> Time，显示当前时间，和系统的 load
C-a K -> kill window，强行关闭当前的 window
```