---
layout: post
title: 在Jekyll的博客中实现语法高亮
date: 2014-06-10 12:55:55
author: scsidisk
category: 文档处理
tags: [Markdown, Jekyll]
---

在Jekyll的博客中实现语法高亮方法如下:

1. 在配置文件 `_config.yml` 启用 pygments, 并使用redcarpet做为渲染引擎.

```yaml
    pygments: true
    markdown: redcarpet
```

2. 使用以下格式把代码括起来

<pre>
    ```python
    print("hello world")
    ```
</pre>

3. 在页面上添加语法高亮的CSS文件, 像这样:

```html
    <link rel="stylesheet" href="/css/pygments.css" />
```

说明:

使用 redcarpet 是为了支持所谓的 [Github风格的Markdown语法](https://help.github.com/articles/github-flavored-markdown),
也就是像上面的那样三撇号加语言类型的方式进行代码引用,
其他几个渲染引擎要用[这种比较别扭的方式](http://jekyllrb.com/docs/templates/#code_snippet_highlighting)来把代码括起来, 我觉得这么写不好看.

语法高亮的定义文件可以在 [这里](https://github.com/richleland/pygments-css/tree/master)下载, 对应的效果可以在 [pygments的官网的Demo](http://pygments.org/demo/)里试看.

本文参考了:
[在Jekyll的博客中实现语法高亮](http://cndenis.github.io/web/2013/11/03/在Jekyll的博客中实现语法高亮/)