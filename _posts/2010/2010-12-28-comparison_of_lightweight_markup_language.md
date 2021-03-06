---
layout: post
title: "轻量标记语言的比较"
date: 2010-12-28 14:55
author: scsidisk
categories: 文档处理
tags: AsciiDoc, Mac
---

轻量标记语言通过简单的文本格式生成复杂样式的文档工具。

[TXT2TAGS](http://txt2tags.sourceforge.net/)
--------------------------------------------

最开始接触的轻量标记语言，是在啄木鸟社区浏览 python 时意外收获的。于自己
使用 markdown 记录心得和写文章之前一直使用 txt2tags。txt2tags 给自己的影
响很好，使自己在庞大而强大的TeX 也有了另外的选择。

-   txt2tags 的语法很简洁，但还是基本可以满足要求，
-   它的一个特点是输出格式非常丰富，有些格式自己还真是很少有机会接触到，这
    也是很体现了此软件的哲学思想：“ont to all”。
-   txt2tags 也支持 css 对输出的html 进行美化，所以 html
    输出还是有很大的自定义性的。

自己而言，不是很喜欢其语法，后来有预见了很偏爱的 markdown，遂弃之。
github 现在还是不支持 txt2tags，当然此为后话。

[ASCIIDOC](http://www.methods.co.nz/asciidoc/)
----------------------------------------------

[LinuxToy](http://linuxtoy.org/) 也有对 asciidoc
做过介绍，自己也有看到过，但当时对
此软件也未有任何的在意。后来也是慢慢的了解轻量标记语言才开始真正的有接触
和使用。

-   asciidoc 理论上可以支持所有常见的文档格式，因为 asciidoc 的后端
    (backend) 为 html 和 docbook。通过 docbook
    作为中间格式，自然可以实现 groff，pdf 等格式的支持。
-   asciidoc
    的功能很完备，可以当作是完整的排版系统，一些功能也很贴心，例 如
    'verse'
    ，很多项目也是选用此作为文档工具，例如[韦诺之战](http://www.wesnoth.org/)，
    [Git](http://git-scm.com/)，[Vimperator](http://vimperator.org/)。
-   asciidoc 的中文支持不好，或者言之为对 utf-8
    支持不佳，但是有相应热心网 友相应的改进版本。另外关于 asciidoc
    的中文介绍和使用也很少，使用还是要 参考文档，但还是很简单的。

RESTRUCTEREDTEXT
----------------

reStructeredText 简称为 rst，是 python 的
[docutils](http://docutils.sourceforge.net/) 的组成部分，可以称 之为
python 的官方文档工具。

在轻量标记语言里，rst 应该可以算是功能最完备的，但是自己不是很习惯，但是
使用起来总是感觉不是很顺手，写 rst 文章都要查阅语法，推荐 pythoner
使用。

github 的 中文 rst 文件渲染还是会乱码，因此现在使用的很少了。

[MARKDOWN](http://daringfireball.net/projects/markdown/)
--------------------------------------------------------

-   markdown 的语法可读性很好，很偏爱 markdown
    的语法，非常的简洁而且也很
    符合自己的偏好。但是比较麻烦的是对于表格没有语法支持，这也是由于
    markdown 自身的哲学所致，其初衷并不是替代并
    html，只是为了提高文件的可
    读性。最近写作文章和记录笔记，经常要插入表格，只能使用内嵌的
    html，对 于不熟悉 html 的自己来说也实在是很折腾。
-   markdown 另一个很吸引自己的是其语言实现非常丰富， perl(原始实现), c,
    java, lua, python, javascript 都有相应的实现，对于 想 hacking
    的自己也是很好的素材。当然，你可以整合这些插件，例如可以把 lua
    的插件整合进 scite
    编辑器，[scite](http://www.scintilla.org/SciTE.html/) 也可以成为
    blog 文章编辑器。
-   github 也可以很好的支持 markdown 格式，这对于自己太方便了。

[ORG](http://orgmode.org/) 与 [MUSE](http://mwolson.org/projects/EmacsMuse.html/)
---------------------------------------------------------------------------------

org 以及 muse(原来的 [emacs-wiki](http://www.emacswiki.org/))
都是只存在于 [emacs](http://www.gnu.org/software/emacs/)
世界的轻量标记语言。 muse 使用很方便而且语法高亮也很强大(依赖于 htmlize
插件)，原来也有使用写 作文章的经历。muse 现在基本慢慢被 org
替代了，虽然它们的初衷不一致。 org 最近发展很快，从 emacs23
开始已经成为了其标准组件，语法与 muse 基本兼容。

-   org 的功能很完善，把它列为轻量标记语言或许也不是太完善。org
    的功能主要 分为文档发布(wiki)或者其本来的 GTD 管理。自己使用 org
    也基本只是用其日 程规划的功能，文档发布功能很少用。在 [emacser
    中文站](http://emacser.com/) 大伙基 本都是用 org
    来撰写文章的，只有自己是用 markdown 的“异类”了，自己太过 偏爱
    markdown 了。
-   “背靠大树好乘凉”，有了 emacs(elisp) 的支持，org
    可拓展性非常好。但是由 于应用只限于 emacs，你在使用 org
    之前还要先进入 emacs 的世界，要付出很
    多的学习代价，但是学习代价应该还是值得的，毕竟 emacs 是
    “神的编辑器”。

SUMMARY
-------

可以参考下
[wiki](http://en.wikipedia.org/wiki/Lightweight_markup_language/)
的比较介绍，看一下众多轻量标记语言的语法差异和输出格
式支持，选择自己喜欢的。

当然，由于轻量标记语言很丰富，因此它们之间的语法相差还是很大。如果经常要
在不同的标记语言之间切换，那大脑的思维转换太累了，那要好好查阅手册了。

自己一直很偏爱
markdown，现在文章写作基本都是用其描述的。而且也可以很方便与 html 整合。


