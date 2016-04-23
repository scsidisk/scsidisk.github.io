---
layout: post
title: "git merge --squash介绍"
date: 2012-07-01
author: scsidisk
categories: Development
tags: Git
---

一般的合并流程如下：

```
$ git merge another
$ git checkout another
# modify, commit, modify, commit ...
$ git checkout master
$ git merge another
```

但是，操作方便并不意味着这样操作就是合理的，在某些情况下，我们应该优先选择使用--squash选项，如下：

```
$ git merge --squash another
$ git commit -m "message here"
```

`--squash` 选项的含义是：本地文件内容与不使用该选项的合并结果相同，但是不提交、不移动HEAD，因此需要一条额外的commit命令。其效果相当于将another分支上的多个commit合并成一个，放在当前分支上，原来的commit历史则没有拿过来。

判断是否使用 `--squash` 选项最根本的标准是，待合并分支上的历史是否有意义。

如果在开发分支上提交非常随意，甚至写成微博体，那么一定要使用 `--squash` 选项。版本历史记录的应该是代码的发展，而不是开发者在编码时的活动。

只有在开发分支上每个commit都有其独自存在的意义，并且能够编译通过的情况下（能够通过测试就更完美了），才应该选择缺省的合并方式来保留commit历史。