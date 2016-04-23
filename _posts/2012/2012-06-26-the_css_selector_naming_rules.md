---
layout: post
title: "Css选择器命名规则"
date: 2012-06-26
author: scsidisk
categories: 前端开发
tags: CSS
---

操作系统版本：Windows 7

浏览器版本：IE6,IE7,IE8,Firefox 3.6.2,Safari 4.0.4,Chrome 5.0.356.2 dev

受影响的浏览器：所有浏览器.

经常讲到css选择器命名规则，其实不只是在团队合作基础上来讲这个，每个浏览器,IE产品，火狐，苹果，谷歌，都会因为命名不规范会产生不同样式..

## 关于选择器的命名

W3C CSS2.1的 4.1.3 节中提到：标识符（包括选择器中的元素名，类和ID）只能包含字符[a- zA-Z0-9]和ISO 10646字符编码U+00A1及以上，再加连字号（-）和下划线（_）；它们不能以 数字，或一个连字号后跟数字为开头。它们还可以包含转义字符加任何ISO 10646字符作为一个数 字编码。

由于设计到的字符很多，本文只针对字符 `[a-zA-Z0-9]` ，再加连字号（-）和下划线（_）进行讨论。 关于CSS中允许使用的字符和大小写信息，请参考 `W3C CSS2.1` 的 `4.1.3` 节

## 差异及可能产生的问题

在W3C CSS2.1说明文档中，只提到选择器标识符不能以数字，或一个连字号后跟数字为开头。除 此之外，没有相关的说明。那么各浏览器下的表现是否遵循这一规则呢？

请观察如下代码：

```html
div{width:160px;height:20px;font-size:12px;line-height:20px;background- color:yellow;}
.f-1_f_{background-color:#d4d4d4;}
.1{background-color:#A8A8A8;}
.123456{background-color:#d4d4d4;}
.2demo{background-color:#A8A8A8;}
.2-demo {background-color:#d4d4d4;}
.2_demo {background-color:#A8A8A8;}
.-demo{background-color:#d4d4d4;}
.-2demo {background-color:#A8A8A8;}
._demo {background-color:#d4d4d4;}
._2demo {background-color:#A8A8A8;}
.-{background-color:#d4d4d4;}
.---{background-color:#A8A8A8;}
._{background-color:#d4d4d4;}
.——{background-color:#A8A8A8;}
._-{background-color:#d4d4d4;}
.-_{background-color:#A8A8A8;}
.-{background-color:#d4d4d4;}
.---_{background-color:#A8A8A8;}
.---123{background-color:#d4d4d4;}
.__123{background-color:#A8A8A8;}
<div class="f-1_f_">字母开头
<div class="1">单个数字
<div class="123456">多个数字
<div class="2demo">数字开头 + [a-z][A-Z]
<div class="2-demo">数字 + "-" 开头
<div class="2_demo">数字 + "_" 开头
<div class="-demo">连字符(-)开头 + [a-z][A-Z]
<div class="-2demo">连字符(-) + 数字 开头
<div class="_demo">下划线(_)开头 + [a-z][A-Z]
<div class="_2demo">下划线(_) + 数字 开头
<div class="-">单个连字符(-)
<div class="\---">多个连字符(-)
<div class="_">单个下划线(_)
<div class=" ">多个下划线(_)
<div class="_-">下划线(_) + 连字符(-)
<div class="-_">连字符(-) + 下划线(_)
<div class=" -">多个下划线(_) + 连字符(-)
<div class="\---_">多个连字符(-) + 下划线(_)
<div class="\---123">多个连字符(-) + 数字
<div class=" 123">多个下划线(_) + 数字
```

看看各浏览器下面的结果

观察上表，分析各浏览器下的表现，总结如下

从上面看到，我们可以直观的了解到选择器的命名在各浏览器下的支持情况有所不同。因此，如果选择器的命名不规范，将影响各浏览器下，样式渲染不一致。比如如下代码：

```html
div{font-size:12px;background-color:yellow;width:150px;height:30px;line- height:30px;}
.20fontsize{font-size:18px;background-color:#d4d4d4;}
<div class="20fontsize">以数字开头的类名
```

以数字开始的类名仅在IE6(Q)/IE7(Q)/IE8(Q)下被识别，而其它浏览器下则不识别（忽略该规则）

## 如何避免受此问题影响

坚持以字母开头命名选择器，这样可保证在所有浏览器下都能兼容。

## 关于团队合作的css命名规范

### 常用的css命名规则

- 头：header
- 内容：content/container
- 尾：footer
- 导航：nav
- 侧栏：sidebar
- 栏目：column
- 页面外围控制整体布局宽度：wrapper
- 左右中：left right center
- 登录条：loginbar
- 标志：logo
- 广告：banner
- 页面主体：main
- 热点：hot
- 新闻：news
- 下载：download
- 子导航：subnav
- 菜单：menu
- 子菜单：submenu
- 搜索：search
- 友情链接：friendlink
- 页脚：footer
- 版权：copyright
- 滚动：scroll
- 内容：content
- 标签页：tab
- 文章列表：list
- 提示信息：msg
- 小技巧：tips
- 栏目标题：title
- 加入：joinus
- 指南：guild
- 服务：service
- 注册：regsiter
- 状态：status
- 投票：vote
- 合作伙伴：partner

### 注释的写法:
- /* Footer */
- 内容区
- /* End Footer */

### id的命名:

#### 页面结构

- 容器: container
- 页头：header
- 内容：content/container
- 页面主体：main
- 页尾：footer
- 导航：nav
- 侧栏：sidebar
- 栏目：column
- 页面外围控制整体布局宽度：wrapper
- 左右中：left right center

#### 导航

- 导航：nav
- 主导航：mainbav
- 子导航：subnav
- 顶导航：topnav
- 边导航：sidebar
- 左导航：leftsidebar
- 右导航：rightsidebar
- 菜单：menu
- 子菜单：submenu
- 标题: title
- 摘要: summary

#### 功能

- 标志：logo
- 广告：banner
- 登陆：login
- 登录条：loginbar
- 注册：regsiter
- 搜索：search
- 功能区：shop
- 标题：title
- 加入：joinus
- 状态：status
- 按钮：btn
- 滚动：scroll
- 标签页：tab
- 文章列表：list
- 提示信息：msg
- 当前的: current
- 小技巧：tips
- 图标: icon
- 注释：note
- 指南：guild
- 服务：service
- 热点：hot
- 新闻：news
- 下载：download
- 投票：vote
- 合作伙伴：partner
- 友情链接：link
- 版权：copyright

### class的命名:

#### 颜色:使用颜色的名称或者16进制代码,如

```
.red { color: red; }
.f60 { color: #f60; }
.ff8600 { color: #ff8600; }
```

#### 字体大小,直接使用”font+字体大小”作为名称,如

```
.font12px { font-size: 12px; }
.font9pt {font-size: 9pt; }
```

#### 对齐样式,使用对齐目标的英文名称,如

```
.left { float:left; }
.bottom { float:bottom; }
```

#### 标题栏样式,使用”类别+功能”的方式命名,如

```
.barnews { }
.barproduct { }
```

### 注意事项::

- 一律小写;
- 尽量用英文;
- 不加中杠和下划线;
- 尽量不缩写，除非一看就明白的单词.

### 其他

- 主要的 master.css
- 模块 module.css
- 基本共用 base.css
- 布局，版面 layout.css
- 主题 themes.css
- 专栏 columns.css
- 文字 font.css
- 表单 forms.css
- 补丁 mend.css
- 打印 print.css
