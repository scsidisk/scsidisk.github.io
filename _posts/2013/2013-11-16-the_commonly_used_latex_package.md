---
layout: post
title: 常用的latex宏包
date: 2013-11-16 21:43
author: scsidisk
category: 文档处理
tags: [Tex, LaTex]
---


CJK 中文支持，现不推荐这种方式
CJKfntef CJK 下的中文下划线、浪线、加点等标记。xeCJK 沿用
CJKpunct CJK 下的中文标点压缩
CJKnumber CJK 下的数字。在 ctex 宏包中被替代为更方便的命令。xeCJK 沿用
CJKspace CJK 下的中西文间距控制
ccmap 使用 PDFLaTeX 处理 CJK 中文文档时的内码修正
xeCJK 用于 LaTeX 的 XeTeX 中文支持，现在推荐这种方式
zhspacing 主要用于 Plain TeX 的 XeTeX 中文支持，LaTeX 下不推荐此方式
ctex, ctexcap 中文文档的一大集格式，必备。可与 CJK、xeCJK、zhspacing
配合
ctexutf8 及其文档类 ctex 系列的 UTF8 版本

beamer 幻灯片最常用的文档类之一

amsmath 数学公式，必备
amssymb 数学字体与符号，必备。注意此宏包已经包括了
amsfonts，不要重复使用
amsthm 数学定理格式控制。也可以考虑用 theorem 宏包
bm 数字粗体。是 \\boldsymbol 和 \\pmb 的新一代替代品。常用
extarrows 可延长上下写文字的箭头、等号等
latexsym 一些符号。功能被 amssymb 代码，不必使用
upgreek 直立体希腊字母
mathrsfs 数学花体
euler 一种数学字体
beton 与 euler 配套的一种正文字体。euler 与 beton 两个字体包合起来就是
concrete 宏包的功能
MinionPro 一个收费的专业字体包，OpenType 格式的部分字体免费
MnSymbol 与 MinionPro 字体配合的数学符号字体
courier 一种常见的打字机（等宽）字体
txfonts Adobe 的 Times 字体及对应的数学字体
pifont 特殊符号字体，常用的是其中的带圈数字。pifont 是 PSNFSS
字体包的一部分
mflogo MetaFont 和 MetaPost 的标志字体
textcomp 注册商标等符号
fontspec XeTeX 下的字体选择宏包。在中文文档中，它会被 xeCJK
宏包调用，不必重复调用

ifpdf 判断编译引擎是否为
pdfTeX，主要对宏包和模板作者有用。很多宏包会自动调用它
ifthen LaTeX 风格的判断语句，主要对宏包和模板作者有用
calc 长度计算功能，主要对宏包和模板作者有用。很多宏包会自动调用它

geometry 设置页面各部分尺寸。常用
fancyhdr 设置页眉页边页脚。常用
multicol 平衡方便的多栏排版
indentfirst 首段缩进。此宏包会被 ctex 系列宏包引用，一般没有必要使用
makeidx 制做索引的基本宏包
tocloft 控制目录格式。与 ctex 宏包有极少量代码冲突，但一般不影响使用
footmisc 控制脚注格式，包括编号、字体、分隔线等
footnpag 使脚注每页更新编号。因为 footmisc
宏包有此功能且更好，建议摒弃此宏包
natbib 方便的参考文献格式控制。常用
hypernat 解决 hyperref 与 natbib 兼容性的宏包，已过时

langtable 跨页的长表格
slashbox 带斜线的表头
multirow 跨长的表格单元格
makecell 许多方便的表格控制，部分可代替 slashbox 宏包功能
hhline 双线表格交点的精细控制。建议不要使用双线，也不要使用此宏包

listings 代码抄录并语法高亮。常用
shortvrb 提供 \\verb 命令的简写形式。此功能 listings 宏包也可提供
verbatim 标准 verbatim
功能的少量扩展，功能不强，主要用其中的大段注释功能。不常用

doc 提供了写 LaTeX 宏包文档需要的一些功能
hyperref 超链接与其他 PDF 专有功能（如表单制做）。常用

float 浮动体的一些格式控制，可以建立新浮动体，此外提供的 H 选项很方便

graphics, graphicx 插图的标准宏包。建议用
graphicx，它是前者方便的语法包装。极常用
epstopdf 在 pdflatex 编译时自动把 eps 文件转换为 pdf 文件。不建议使用
xcolor, color 颜色支持。常用。xcolor 比 color
功能更多，主要提供了更方便的混色语法。此宏包会被许多图形相关的宏包自动调用
tikz 功能最强大的绘图宏包之一
pict2e 标准 LaTeX picture 环境的扩展，去掉了原有的一些限制。一般仅在与
slashbox 等宏包配合时使用，不推荐使用
