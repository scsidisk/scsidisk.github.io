---
layout: post
title: "editconfig 统一编辑器配置"
date: 2014-08-16
author: scsidisk
categories: IT
tags: IT
---

在项目开发过程中，有的人喜欢用tab来缩进，有的人喜欢用空格。怎样保持缩进风格的一致，缩进大小，tab长度以及字符集等。

以前先发规范，然后逐一沟通，花费精力去做的事情，现在简单多了，每个项目包含一个.editorconfig 文件，顿时世界清净了。

EditorConfig用户来规范编辑器的设置，Editorconfig项目由两部分组成，一个是.editorconfig 的文件格式（format）,一个是editorconfig 插件（plugin）

可以在同一个项目中设置不同的脚本使用不同的风格。

## editorconfig 文件

把 .editorconfig 放在项目根目录下， 当打开文件的时候，editorconfig 插件就会在当前目录及上级目录寻找 .editorconfig 文件。

官方的例子，可以根据自身的情况进行修改

```
# EditorConfig is awesome: http://EditorConfig.org

# top-most EditorConfig file
root = true

# Unix-style newlines with a newline ending every file
[*]
end_of_line = lf
insert_final_newline = true

# 4 space indentation
[*.py]
indent_style = space
indent_size = 4

# Tab indentation (no size specified)
[*.js]
indent_style = tab

# Indentation override for all JS under lib directory
[lib/**.js]
indent_style = space
indent_size = 2

# Matches the exact files either package.json or .travis.yml
[{package.json,.travis.yml}]
indent_style = space
indent_size = 2
```

## editconfig的插件

到官网[下载插件](http://editorconfig.org/#download)， 目前支持下列编辑器.

- AppCode
- Atom
- Code::Block
- Emacs
- Geany
- Gedit
- intelliJ
- jEdit
- Notepad++
- PHPStorm
- PyCharm
- RubyMine
- [Sublime Text](https://github.com/sindresorhus/editorconfig-sublime#readme)
- TextMate
- [Vim](https://github.com/editorconfig/editorconfig-vim#readme)
- Visual Studio
- WebStorm

