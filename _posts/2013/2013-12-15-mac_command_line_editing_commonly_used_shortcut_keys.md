---
layout: post
title: Mac 命令行下编辑常用的快捷键
date: 2013-12-15
author: scsidisk
category: MacOSX
tags: MacOSX
---

本文讲述了 Mac 命令行编辑快捷键的方法，希望对您有所帮助。

## Mac 命令行命令

```
history 显示命令历史列表
[Tab] =命令行自动补全
↑(Ctrl+p) 显示上一条命令
↓(Ctrl+n) 显示下一条命令
clear 清除 shell 提示屏幕
exit 注销
history 显示命令历史
reset 刷新 shell 提示屏幕
```

## Mac 命令行编辑快捷键

```
↑(Ctrl+p) 显示上一条命令
↓(Ctrl+n) 显示下一条命令
!num 执行命令历史列表的第num条命令
!! 执行上一条命令
!?string? 执行含有string字符串的最新命令
Ctrl+r 然后输入若干字符，开始向上搜索包含该字符的命令，继续按Ctrl+r，搜索上一条匹配的命令
Ctrl+s 与Ctrl+r类似,只是正向检索
Ctrl+f 光标向前移动一个字符,相当与->
Ctrl+b 光标向后移动一个字符,相当与<-
opt+<\- 光标向前移动一个单词
opt+-> 光标向后移动一个单词
ls !$ 执行命令ls，并以上一条命令的参数为其参数
Ctrl+a 移动到当前行的开头
Ctrl+e 移动到当前行的结尾
Esc+b 移动到当前单词的开头
Esc+f 移动到当前单词的结尾
Ctrl+l 清屏
Ctrl+u 剪切命令行中光标所在处之前的所有字符（不包括自身）
Ctrl+k 剪切命令行中光标所在处之后的所有字符（包括自身）
Ctrl+d 删除光标所在处字符
Ctrl+h 删除光标所在处前一个字符
Ctrl+y 粘贴刚才所删除的字符
Ctrl+w 剪切光标所在处之前的一个词（以空格、标点等为分隔符）
Ctrl+t 颠倒光标所在处及其之前的字符位置，并将光标移动到下一个字符
Ctrl+v 插入特殊字符,如Ctrl+v+Tab加入Tab字符键
Esc+t 颠倒光标所在处及其相邻单词的位置
Ctrl+c 删除整行
Ctrl+(x u) 按住Ctrl的同时再先后按x和u，撤销刚才的操作
Ctrl+s 挂起当前shell
Ctrl+q 重新启用挂起的shell
```

## 下面的应用可能稍稍高级一点点

```
# !! - 上一条命令
# !-n - 倒数第N条历史命令
# !-n:p - 打印上一条命令（不执行）
# !?string？- 最新一条含有“string”的命令
# !-n:gs/str1/str2/ - 将倒数第N条命令的str1替换为str2，并执行（若不加g,则仅替换第一个）
```
