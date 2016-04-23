---
layout: post
title: "MacTeX 2012 中 CJK 宏包的配置"
date: 2013-10-30
author: scsidisk
categories: 文档处理
tags: MacTeX, MacOSX, CJK, Tex
---

不知道出于什么原因，比较经典的 CJK 宏包在 MacTeX 2012 中基本被丢弃。但是，我们可能会有一些用 CJK 宏包写的相对古老的 GBK 文档。很明显，与其逐个修改 LaTeX 文档，倒不如修改 MacTeX 来得一劳永逸。下面就来谈谈如何让 CJK 宏包工作起来？


首先，MacTeX 中已包含了 CJK、Zhmetrics 等处理中文的宏包，只不过它们主要被用作 CTeX 宏包的 XeLaTeX 组成部分；自身的功能则被肢解了，现在就来给出我们的还原方法。

想让 CJK 宏包使用系统字体，需要将系统的字体目录路径写入 OSFONTDIR 变量。通常，我们可以在 TEXHOME 树中的 texmf.cnf 文件中修改该变量，具体如下：

```
$ cd /usr/local/texlive/2012/
$ sudo vim texmf.cnf
```

修改 OSFONTDIR 为

```
OSFONTDIR = /Library/Fonts//;/System/Library/Fonts//;~/Library/Fonts//
```

接着需要为 CJK 宏包的相关字体增加对应的字体 map 文件。由于我们要使用 LaTeX 编译文件，因此，需要为 ttf2pk、dvipdfmx 增加 map 文件，这些文件本身就在 MacTeX 之中，我们只需要把它们找出来作适当的修改即可。作如下操作：

```bash
$ mkdir -p ~/Library/texmf/dvipdfmx/
$ cp /usr/local/texlive/2012/texmf-dist/dvipdfmx/dvipdfmx.cfg ~/Library/texmf/dvipdfmx/dvipdfmx.cfg
```

打开该文件并去掉下面这行的注释符号（%）:

```
$ vim ~/Library/texmf/dvipdfmx/dvipdfmx.cfg

f  cid-x.map
```

还需要修改 cid-x.map，并在其中增加中易公司字体的内容：

```
$ cp /usr/local/texlive/2012/texmf/fonts/map/dvipdfmx/cid-x.map ~/Library/texmf/fonts/map/dvipdfmx/cid-x.map

$ cat cid-x.map | grep -i sim
%%    The remap option [-r] is simply ignored.
gbk@UGBK@ unicode :0:simsun.ttc -v 50
gbksong@UGBK@ unicode :0:simsun.ttc -v 50
gbkkai@UGBK@ unicode simkai.ttf -v 70
gbkhei@UGBK@ unicode simhei.ttf -v 150
gbkfs@UGBK@ unicode simfang.ttf -v 50
gbkli@UGBK@ unicode simli.ttf -v 150
gbkyou@UGBK@ unicode simyou.ttf -v 60
unisong@Unicode@ unicode :0:simsun.ttc -v 50
unikai@Unicode@ unicode simkai.ttf -v 70
unihei@Unicode@ unicode simhei.ttf -v 150
unifs@Unicode@ unicode simfang.ttf -v 50
unili@Unicode@ unicode simli.ttf -v 150
uniyou@Unicode@ unicode simyou.ttf -v 60
gbksongsl@UGBK@ unicode :0:simsun.ttc -s .167 -v 50
gbkkaisl@UGBK@ unicode simkai.ttf -s .167 -v 70
gbkheisl@UGBK@ unicode simhei.ttf -s .167 -v 150
gbkfssl@UGBK@ unicode simfang.ttf -s .167 -v 50
gbklisl@UGBK@ unicode simli.ttf -s .167 -v 150
gbkyousl@UGBK@ unicode simyou.ttf -s .167 -v 60
unisongsl@Unicode@ unicode :0:simsun.ttc -s .167 -v 50
unikaisl@Unicode@ unicode simkai.ttf -s .167 -v 70
uniheisl@Unicode@ unicode simhei.ttf -s .167 -v 150
unifssl@Unicode@ unicode simfang.ttf -s .167 -v 50
unilisl@Unicode@ unicode simli.ttf -s .167 -v 150
uniyousl@Unicode@ unicode simyou.ttf -s .167 -v 60
```

剩下来还需要为 ttf2pk 增加 map 文件。这个很简单，只需要从 MacTeX 中复制文件即可：

```
$ mkdir -p ~/Library/texmf/fonts/map/ttf2pk/config
$ cp /usr/local/texlive/2012/texmf-dist/source/fonts/zhmetrics/ttfonts.map ~/Library/texmf/fonts/map/ttf2pk/config/ttfonts.map
```

为了让 MacTeX 系统能够找到我们修改过的文件，通常都需要刷新目录树数据文件：

```bash
$ mktexlsr
$ updmap
```

最后，为了确保字体文件能被找到，请打开字体册 FontBook，查看中易六套字体文件。如果没有的话，请安装 MS Office for Mac；当然，也可以在获得使用这些字体的授权之后，将它们从 Windows 系统复制到 Mac OS 的字体目录 /Library/Fonts 或~/Library/Fonts中。

为了说明我们的修改已经成功，现做如下的测试文件，打开 TeXShop，并录入下面的内容：

```
%!TEX TS-program = latex
%!TEX encoding = GBK

%% test.tex
%% a sample file for testing CJK Package

\documentclass{article}
\usepackage{CJK}
\begin{document}
\begin{CJK*}{GBK}{song}
你好!
\end{CJK*}
\end{document}
```

在 TeXShop 中点击 typeset 按钮，大功告成。