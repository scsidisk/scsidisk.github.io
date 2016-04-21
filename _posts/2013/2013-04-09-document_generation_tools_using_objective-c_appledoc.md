---
layout: post
title: 使用Objective-C的文档生成工具:Appledoc
date: 2013-04-09
author: scsidisk
category: 移动开发
tags: [ObjC]
---

## 前言

做项目的人多了，就需要文档了。今天开始尝试写一些项目文档。但是就源代码来说，文档最好和源码在一起，这样更新起来更加方便和顺手。象Java语言本身就自带javadoc命令，可以从源码中抽取文档。今天抽空调研了一下objective-c语言的类似工具。

从stackoverflow 上找到三个比较popular的工具：doxygen, headdoc和appledoc 。它们分别的官方网址如下：

- docxygen <http://www.stack.nl/~dimitri/doxygen/index.html>
- headdoc <http://developer.apple.com/opensource/tools/headerdoc.html>
- appledoc <http://gentlebytes.com/appledoc/>

## 介绍

我把这3个工具都大概调研了一下，说一下我的感受。

### docxygen

docxygen感觉是这3个工具中支持语言最多的，可以配置的地方也比较多。我大概看了一下文档，觉得还是比较复杂，而且默认生成的风格与苹果的风格不一致。就去看后面2个工具的介绍了。另外，它虽然是开源软件，但是没有将源码放到github上让我感觉这个工具的开发活跃度是不是不够。

### headerdoc

headerdoc是xcode 自带的文档生成工具。在安装完xcode后，就可以用命令行：headdoc2html + 源文件名 来生成对应的文档。我个人试用了一下，还是比较方便的，不过headerdoc的注释生成规则比较特别，只生成以 /*! */ 的格式的注释。还有一个缺点是每个类文件对应一个注释文件，没有汇总的文件，这点感觉有点不爽。

### Appledoc

appledoc是在stackoverflow上被大家推荐的一个注释工具。有几个原因造成我比较喜欢它：

- 它默认生成的文档风格和苹果的官方文档是一致的，而doxygen需要另外配置。
- appledoc就是用objective-c生成的，必要的时候调试和改动也比较方便。
- 可以生成docset，并且集成到xcode中。这一点是很赞的，相当于在源码中按住option再单击就可以调出相应方法的帮助。
- appledoc源码在github上，而doxygen在svn上。我个人比较偏激地认为比较活跃的开源项目都应该在github上。
- 相对于headerdoc，它没有特殊的注释要求，可以用/** */ 的格式，也可以兼容/*! */的格式的注释，并且生成的注释有汇总页面。

## 安装

```
git clone git://github.com/tomaz/appledoc.git
cd appledoc
sudo sh [install-appledoc.sh](http://install-appledoc.sh)
```

那么简单介绍一下如何安装appledoc，安装非常简单，只需要2步：

## 使用

使用appledoc时，只需要用如下命令即可：

```
appledoc -o ./doc --project-name ynote --project-company youdao .
```

appledoc会扫描当前路径下的所有文件，然后生成好文档放到doc目录下。你也可以用appledoc –help查看所有可用的参数。

基本上使用起来还是比较方便的，详细的信息可以查看官方的文档：http://gentlebytes.com/appledoc/


