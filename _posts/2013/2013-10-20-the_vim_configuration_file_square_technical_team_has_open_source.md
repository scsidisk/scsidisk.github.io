---
layout: post
title: Square 技术团队的 Vim 配置文件已开源
date: 2013-10-20 11:32
author: scsidisk
category: VIM
tags: [Linux, Mac, VIM]
---

（译自知名移动支付公司Square官博8月28日的文章）Square 的工程师在使用很多种代码编辑器：Sublime、IntelliJ、Xcode 和 Vim。其中 Vim 是使用最多的，随着时间推移，在 Square 的 Vim 粉丝把配置、快捷方式和插件汇编成一个单独的仓库，我们亲切地称为 Maximum Awesome，并把它开源了。我们希望其他在用 OS X 的朋友能够在几分钟之内就能用上 Vim。

Maximum Awesome 配备了很多完整 IDE 有的特性：语法高亮、代码补全、错误高亮等等。下面这些是我喜欢的快捷方式和插件：

- 共享剪贴板：Vim中的寄存器（register，作用和Windows中的剪贴板类似）和 OS X 剪贴板同步，可以像在本地应用中移动代码。
- Command-T 插件：对于那些用Sublime或TextMate的朋友来说，这个已经很熟悉了。不过在Vim中，使用快捷方式,t ，后面加你想打开的文件名。
- NERDTree 插件：浏览项目的文件结构、移动文件或创建新文件，想做这些操作，都不要离开舒适的Vim啦。使用 ,d 打开 drawer，或使用 ,f 来给当前文件开启NERDTree。
- 集成 Git：这个插件包括了大多数的 git 命令，但是我最喜欢的是 :Gblame 和 :Gdiff。用 :Gblame 可知道谁写某个文件的不同部分，用 :Gdiff ，可以在两个侧栏中对比我刚才写的内容
- 快速注释代码：用 \\\\\\ 可快速注释某一行代码，或者用 \\\ 注释选中的代码

当然了，这里还有些不是 Vim 的组件了。Maximum Awesome 搭配了iTerm 2、一个 tmux 配置文件，还有 Solarized color scheme.。详细内容，请 Vim 爱好者移步：<https://github.com/square/maximum-awesome>

原文链接： [Riley Strong](http://corner.squareup.com/2013/08/fly-vim-first-class.html)   
译文链接：<http://blog.jobbole.com/46966/>
