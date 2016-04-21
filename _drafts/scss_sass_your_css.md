---
layout: post
title: SASS用法指南
date: 2016-04-10
author: scsidisk
category: CSS
tags: CSS, SCSS
---

SASS
-----

SASS是一种CSS的开发工具, 提供了许多便利的写法, 大大节省了设计者的时间, 使得CSS的开发, 变得简单和可维护。
本文总结了SASS的主要用法。我的目标是, 有了这篇文章, 日常的一般使用就不需要去看官方文档了。

安装
-----

　　gem install sass

文件类型
--------

sass有两种后缀名文件：一种后缀名为sass, 不使用大括号和分号；另一种就是我们这里使用的scss文件, 这种和我们平时写的css文件格式差不多, 使用大括号和分号。目前所有sass文件都指后缀名为scss的文件。在此也建议使用后缀名为scss的文件, 以避免sass后缀名的严格格式要求报错。

两种文件通过 sass-convert 命令行工具可以互相转换：

    # 将 SCSS 转换为 Sass
    $ sass-convert style.scss style.sass
    # 将 Sass 转换为 SCSS
    $ sass-convert style.sass style.scss

导入
------

sass 的导入 (@import) 规则和CSS的有所不同, 编译时会将 @import 的 scss 文件内容合并进来。导入 css 文件如 @import 'reset.css', css 文件不会合并到编译后的文件中, 而是以 @import 方式存在。

所有的 sass 导入文件都可以忽略后缀名 `.scss` 。一般来说基础的文件命名方法以 `_` 开头, 如 `_mixin.scss`。这种文件在导入的时候可以不写下划线, 可写成 `@import "mixin"`。

用法
----

SASS文件就是普通的文本文件, 里面可以直接使用CSS语法。

    # 转换并显示
　　sass test.scss
    # 转换成 css 文件
　　sass test.scss test.css

SASS提供四个编译风格的选项：

　　* nested：嵌套缩进的css代码, 它是默认值。
　　* expanded：没有缩进的、扩展的css代码。
　　* compact：简洁格式的css代码。
　　* compressed：压缩后的css代码。

生产环境当中, 一般使用最后一个选项。

　　sass --style compressed test.sass test.css
    # 開啟debug模式 (記得要先刪原本編譯出的css)
    sass --watch --debug-info style.scss:style.css

你也可以让SASS监听某个文件或目录, 一旦源文件有变动, 就自动生成编译后的版本。

　　// watch a file
　　sass --watch input.scss:output.css
　　// watch a directory
　　sass --watch app/sass:public/stylesheets

三、基本语法

- 变量

SASS允许使用变量, 所有变量以$开头。

　　$blue : #1875e7;　
　　div {
　　　color : $blue;
　　}

如果变量需要镶嵌在字符串之中, 就必须需要写在#{}之中。

　　$side : left;
　　.rounded {
　　　　border-#{$side}-radius: 5px;
　　}

默认变量, sass的默认变量仅需要在值后面加上!default即可。

sass 的默认变量一般是用来设置默认值, 然后根据需求来覆盖的, 覆盖的方式也很简单, 只需要在默认变量之前重新声明下变量即可

    $baseLineHeight:        2;
    $baseLineHeight:        1.5 !default;
    body{
        line-height: $baseLineHeight;
    }


- 计算功能

SASS允许在代码中使用算式：

　　body {
　　　　margin: (14px/2);
　　　　top: 50px + 100px;
　　　　right: $var * 10%;
　　}

- 嵌套

SASS允许选择器嵌套。比如, 下面的CSS代码：

　　div h1 {
　　　　color : red;
　　}
可以写成：

　　div {
　　　　hi {
　　　　　　color:red;
　　　　}
　　}

属性也可以嵌套, 比如border-color属性, 可以写成：

　　p {
　　　　border: {
　　　　　　color: red;
　　　　}
　　}

注意, border后面必须加上冒号。

在嵌套的代码块内, 可以使用&引用父元素。比如a:hover伪类, 可以写成：

　　a {
　　　　&:hover { color: #ffb3ff; }
　　}

-  注释

SASS共有两种注释风格。

标准的CSS注释 /* comment */ , 会保留到编译后的文件。

单行注释 // comment, 只保留在SASS源文件中, 编译后被省略。

在/*后面加一个感叹号, 表示这是"重要注释"。即使是压缩模式编译, 也会保留这行注释, 通常可以用于声明版权信息。

　　/*!
　　　　重要注释！
　　*/

- 继承

SASS允许一个选择器, 继承另一个选择器。比如, 现有class1：

　　.class1 {
　　　　border: 1px solid #ddd;
　　}

class2要继承class1, 就要使用@extend命令：

　　.class2 {
　　　　@extend .class1;
　　　　font-size:120%;
　　}

- Mixin

Mixin有点像C语言的宏（macro）, 是可以重用的代码块。

使用@mixin命令, 定义一个代码块。

　　@mixin left {
　　　　float: left;
　　　　margin-left: 10px;
　　}

使用@include命令, 调用这个mixin。

　　div {
　　　　@include left;
　　}

mixin的强大之处, 在于可以指定参数和缺省值。

　　@mixin left($value: 10px) {
　　　　float: left;
　　　　margin-right: $value;
　　}

使用的时候, 根据需要加入参数：

　　div {
　　　　@include left(20px);
　　}

下面是一个mixin的实例, 用来生成浏览器前缀。

　　@mixin rounded($vert, $horz, $radius: 10px) {
　　　　border-#{$vert}-#{$horz}-radius: $radius;
　　　　-moz-border-radius-#{$vert}#{$horz}: $radius;
　　　　-webkit-border-#{$vert}-#{$horz}-radius: $radius;
　　}

使用的时候, 可以像下面这样调用：

　　#navbar li { @include rounded(top, left); }
　　#footer { @include rounded(top, left, 5px); }

- 颜色函数

SASS提供了一些内置的颜色函数, 以便生成系列颜色。

　　lighten(#cc3, 10%) // #d6d65c
　　darken(#cc3, 10%) // #a3a329
　　grayscale(#cc3) // #808080
　　complement(#cc3) // #33c

- 插入文件

@import命令, 用来插入外部文件。

　　@import "path/filename.scss";

如果插入的是.css文件, 则等同于css的import命令。

　　@import "foo.css";

高级用法
--------

- 条件语句

@if可以用来判断：

　　p {
　　　　@if 1 + 1 == 2 { border: 1px solid; }
　　　　@if 5 < 3 { border: 2px dotted; }
　　}

配套的还有@else命令：

　　@if lightness($color) > 30% {
　　　　background-color: #000;
　　} @else {
　　　　background-color: #fff;
　　}

- 循环语句

SASS支持for循环：

　　@for $i from 1 to 10 {
　　　　.border-#{$i} {
　　　　　　border: #{$i}px solid blue;
　　　　}
　　}

也支持while循环：

　　$i: 6;
　　@while $i > 0 {
　　　　.item-#{$i} { width: 2em * $i; }
　　　　$i: $i - 2;
　　}

each命令, 作用与for类似：

　　@each $member in a, b, c, d {
　　　　.#{$member} {
　　　　　　background-image: url("/image/#{$member}.jpg");
　　　　}
　　}

- 自定义函数

SASS允许用户编写自己的函数。

　　@function double($n) {
　　　　@return $n * 2;
　　}
　　#sidebar {
　　　　width: double(5px);
　　}

