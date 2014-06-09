---
layout: post
title: \[原创\]safari阅读器模式字体调整
date: 2010-12-28 14:55
author: scsidisk
category: MacOSX
tags: Font, Mac, Safari
Slug: safari_reader_mode_adjust_font_size
---

一直使用safari，感觉还是不错的，最近升级到safari5.0，发现阅读器的功能不错，不过字体显示优先为宋体，可能是我的机器安装有宋体的缘故。

今天设置了一下safari的阅读器模板，吧字体设置成了黑体。

右击safari-\>显示包内容-\>Contents-\>Resources-\>使用文本编辑器打开Reader.html

14行处  
h1.title {  
font-family: Palatino, Georgia, Times, "Times New Roman", serif;

改为  
h1.title {  
font-family: "Lucida Grande","华文细黑","STHeiti" !important;

82行处  
.page {  
font: 20px Palatino, Georgia, Times, "Times New Roman", serif;  
改为  
.page {  
font: 20px "Lucida Grande","华文细黑","STHeiti" !important;

 

<div class="posttagsblock">
</div>

