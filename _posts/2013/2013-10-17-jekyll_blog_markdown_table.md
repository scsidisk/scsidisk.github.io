---
layout: post
title: 在Jekyll的博客中使用表格
date: 2013-10-17
author: scsidisk
category: Markdown
tags: Markdown, Jekyll
---

看到人家在 Markdown 文件中随意的书写表格，但在我自己的博客中却怎么也无法输出表格样式，最后还是通过 Google 才找到答案，需要语法解释引擎 Redcarpet，且开启 tables 选项。

在 Jekyll 中使用，请修改 _config.yml

```
markdown: redcarpet
redcarpet: 
    extensions: ["tables"]
```

随后

```
$ gem install redcarpet
```

在 Markdwon 文件中可以依据以下语法进行书写

```
|head1|head2|head3|head4|
|---|:---|:---:|---:|
|row1text1|row1text3|row1text3|row1text4|
|row2text1|row2text3|row2text3|row2text4|
```

其中 :所在位置表示表格的位置对齐

其最后输出的代码是

```
<table>
  <thead>
    <tr>
      <th>head1 head1 head1</th>
      <th align="left">head2 head2 head2</th>
      <th align="center">head3 head3 head3</th>
      <th align="right">head4 head4 head4</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>row1text1</td>
      <td align="left">row1text3</td>
      <td align="center">row1text3</td>
      <td align="right">row1text4</td>
    </tr>
    <tr>
      <td>row2text1</td>
      <td align="left">row2text3</td>
      <td align="center">row2text3</td>
      <td align="right">row2text4</td>
    </tr>
  </tbody>
</table>
```

redcarpet有很多选项可以开启，譬如我就开启了

```
markdown: redcarpet
redcarpet: 
    extensions: ["fenced_code_blocks", "tables", "highlight", "with_toc_data", "strikethrough", "underline"]
```