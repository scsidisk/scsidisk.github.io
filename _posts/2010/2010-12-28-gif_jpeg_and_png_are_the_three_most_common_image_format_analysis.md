---
layout: post
title: "GIF、JPEG 和 PNG 是三种最常见的图片格式分析。"
date: 2010-12-28 14:54
author: scsidisk
categories: Linux
tags: gif, jpeg, png
Slug: gif_jpeg_and_png_are_the_three_most_common_image_format_analysis
---

###导读

GIF、JPEG 和 PNG 是三种最常见的图片格式分析。 GIF：1987 年诞生，常用于网页动画，使用无损压缩，支持 256 种颜色（一般叫 8 bit 彩色），支持单一透明色； JPEG：1992 年出世，照片一般

###GIF、JPEG 和 PNG 是三种最常见的图片格式分析

GIF：1987 年诞生，常用于网页动画，使用无损压缩，支持 256 种颜色（一般叫 8 bit 彩色），支持单一透明色；

JPEG：1992 年出世，照片一般都用这个格式，有损压缩，24 bit 真彩色（224 = 17 万种颜色），不支持动画，不支持透明色；

PNG：1996 年问世，无损压缩，最常见的使用格式是 256 索引色（PNG-8）和 24 bit 真彩色（PNG-24）（当然 PNG 支持的颜色格式远不止此），支持 full alpha 通道（256 级可调半透明色），不支持动画。

###简单比较

JPEG v.s. PNG：JPEG 在照片压缩方面拥有巨大的优势，这方面无可替代，但是 JPEG 是有损压缩，图片质量会有损失。另外，一般屏幕截屏用 PNG 格式不但比 JPEG 质量高而且文件大小还更小（维基有图）。

GIF v.s. PNG：GIF 只在简单动画领域有优势（其实，GIF 256 色限制以及无损压缩机制导致高质量的动画的发布一般都使用 Flash 等格式），只要没有动画，PNG 完全可以取代 GIF。

防锯齿：下面是 GIF 和 PNG 防锯齿处理的对比，六张小图片是分别放到浅黄和深绿背景下的情景，三张大图是深绿背景情形的放大。由于 GIF 没有半透明一说，所以防锯齿处理时只能假设背景是白色，这样的 GIF 放在深色背景下还不如不防锯齿。而 PNG 图片可以轻松应付各种背景颜色，特别适合用来做网页和应用程序里的通用防锯齿图标适应不同皮肤，没有 full alpha 通道的 JPEG 和 GIF 都做不到这一点。

[![ren](/static/images/2010/12/5_100106212941_1.png)](/static/images/2010/12/5_100106212941_1.png)

可以看到，除了照片和动画，PNG 是最好的格式，但是 PNG 为什么到最近几年才流行起来？有很多原因：

PNG 诞生的时候互联网已经初具规模，当时 GIF 和 JPEG 已经是很流行的格式了，换格式的迁移成本是很大的，有时候惯性是一个很可怕的东西。

浏览器的 PNG 支持比较落后，比如 IE 就是到 IE4 才开始支持 PNG。

PNG 当初标准里把 alpha 通道写成了 optional 的，土鳖的 IE 一直到 IE7才开始支持 full alpha 通道。而一般网页图标 256 色足够，所以为了支持IE6，PNG 相对于 GIF 毫无优势可言，更何况 GIF 还支持动画。

尽管从原理上说，同样质量的 PNG 图片文件一般要比 GIF要小，但是早期很多图片编辑器不支持 PNG，甚至支持的也没有完全利用 PNG 压缩算法里最精妙的部分（最典型的例子就是早期的 Photoshop），保存出来的 PNG 往往巨大无比。现在的软件已经没有这些问题了，但是这个偏见还广泛存在。

