---
layout: post
title: Mac 安装 Golang 开发环境
date: 2015-11-03
author: scsidisk
category: Golang
tags: Mac Sublime Go Golang
---

golang的官方网站是： https://golang.org/

### 安装golang

    brew update
    brew install golang

接下来我们还要设置环境变量`GOPATH`和`GOBIN`，GOPATH
是我们日常开发的根目录，GOBIN是GOPATH下的bin目录。且需要将GOBIN目录加入到PATH目录后面，生成的可执行文件就可以直接执行了。

    vim ~/.bash_profile

添加下面内容：

    export GOPATH=$HOME/go
    export GOBIN=$GOPATH/bin
    export PATH=$PATH:$GOBIN

至此，我们的go语言已经安装好了。 运行

    go env

应该能看到 golang 的环境变量

### 安装gocode

gocode是go语言语法提示的工具，可以集成到sublime、eclipse等编辑器和ide中，为我们的编码提供便利的提示。

    go get github.com/nsf/gocode
    go install github.com/nsf/gocode

下载的代码存放在`GOPATH`下的src目录下，编译后的可执行文件放在了`GOPATH`下的bin目录下。

###  为 Sublime 安装 GoSublime 插件

`GoSublime`是直接在sublime里运行go程序的插件，

安装好GoSublime之后，如果你的环境变量`GOPATH`、`GOPATH`等没有设置好，或者要使用一个不一样的配置，可以打开Preferences -\> Package Settings -\> GoSublime -\> Settings - user，按照下面的格式，填写你的配置内容：

    {
        "env": {
            "GOROOT" : "/path/to/go/",
            "GOPATH" : "/path/to/gopath",
            "GOBIN" : "/path/to/go/bin",
        }
    }

<!-- ### 为 Sublime 安装 GoGDB 插件

从菜单中打开 Perferences->Package Settings->GoGDB->Settings-User ，

会打开GoGDB的settings文件，找到"workingdir"和"commandline"所在位置，

指定好开放项目的路径及执行文件名称，如下：

    {
        "go_cmd": "/usr/local/bin/go",
    }

### 安装 gdb

无 -->

### 编写 go 语言版本的 hello world

在`GOPATH`目录下的src目录下，建立一个hello文件夹，然后创建main.go文件，填写如下内容：

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello World!");
}
```

在sublime中，按command + b 打开shell命令行，输入如下命令执行：

    go run main.go

正常情况下会输出：

    [ `go run main.go` | done: 401.796524ms ]
      Hello World!

到目前为止，我们已经安装好了开发golang程序的基本环境.

### 参考

- http://tonybai.com/2014/10/21/organize-golang-code/
- http://tabalt.net/blog/install-golang-env-on-mac/