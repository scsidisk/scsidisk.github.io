---
layout: post
title: "关于vim中undo/redo的brances研究"
date:   2010-12-28 13:51
categories: VIM
tags: CentOS, Linux, Mac, Shell, Vim
---

用来用去还是vim用着顺手，对于undo/redo做了番研究，其实除了u和ctrl+r之外，vim里有一个brances的概念。

一般的undo和redo都是在主线上操作。假设你undo了一些输入，然後修改文本，
这时候会产生brance。在其他没有brances概念的编辑器里，你就没办法回到第一次的最後修改。但是在vim里，你可以通过g+，g-来进入那个
brances。

具体可以看下面的分支图

[![1080714\_0500](/images/2010/12/1080714_0500.jpg)](/images/2010/12/1080714_0500.jpg)


