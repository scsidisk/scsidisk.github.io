---
layout: post
title: "vi 使用技巧 copy, paste, delete, 块编辑，redo/undo"
date: 2010-12-28 13:51
author: scsidisk
categories: VIM
tags: CentOS, Linux, Mac, Shell, Vim
Slug: vi_tips_copy_paste_delete_block_editing_redo__undo
---

​1. copy and paste

yy : copy 光标所在的行

nyy: copy n line

yw: copy 光标所在的单词

nyw: copy 光标所在位置到其后的n 个单词(未必是同一行)

y\$: copy 光标所在位置到行尾(\$是行尾的标示)

ny\$: copy 光标所在位置之后的n行(包括当前行，当前行=y\$)

p: paste 在光标所在位置之右

P: paste 在光标所在位置之左

​2. delete, 和copy 类似

dd : delete current line

ndd: delete n line

dw: delete current word

ndw: delete n word

d\$ : delete to the end of line.

nd\$ : delete n line. (current line = d\$)

x: delete one character(无论是ascii 还是unicode)

nx: delete n characters.

​3. block edit

在命令模式下，输入v 进入块编辑状态

​a. 移动光标选定操作快

​b. c(cut), y(copy)

​c. p or P.

​4. undo /redo

u: undo

U: 取消最近一行的改动

crtl +r: redo

e!: 放弃所有改动，重新编辑。(王朝网络 wangchao.net.cn)

<div class="posttagsblock">
</div>

