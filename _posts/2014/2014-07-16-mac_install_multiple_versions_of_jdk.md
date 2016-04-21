---
layout: post
title: Mac下使用jenv管理多个版本的JDK
date: 2014-07-16
author: scsidisk
category: Java
tags: [Java, Jenv]
---

jenv 可以在OS X中使用jenv管理多个Java版本, 没有需求的可以忽略此文。

## 一、gcuisinier 版本 (像 rbenv)

官方网站: http://www.jenv.be/

能管理 java 版本, 可以通过 brew 安装, 本人比较喜欢。

### 安装

```
$ brew install jenv
```

### 配置

使用Bash的情况

```
$ echo 'export PATH="$HOME/.jenv/bin:$PATH"' >> ~/.bash_profile
$ echo 'eval "$(jenv init -)"' >> ~/.bash_profile
```

### 查看已经安装的Java版本

```
$ jenv versions
* system (set by /Users/bxpeng/.jenv/version)
```

上面 `*` 表示当前选择的版本。

和rbenv不同的是, jenv不能自己安装任何版本的Java, 所以需要我们手动安装好之后再用jenv指向它们。

1. 安装Java 6, 从 [Apple 网站](https://support.apple.com/kb/DL1572) 下载。它将安装到/System/Library/Java/JavaVirtualMachines/
1. 安装Java 7, 从 [Oracle 网站](http://www.oracle.com/technetwork/cn/java/javase/downloads/jdk7-downloads-1880260.html) 下载.它将安装到/Library/Java/JavaVirtualMachines/
1. 安装Java 8, 从 [Oracle 网站](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 下载.它将安装到/Library/Java/JavaVirtualMachines/

使用 jenv add 将安装的 Java 加入 jenv 中

```
$ jenv add /System/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home/
$ jenv add /Library/Java/JavaVirtualMachines/jdk1.7.0_71.jdk/Contents/Home/
$ jenv add /Library/Java/JavaVirtualMachines/jdk1.8.0_25.jdk/Contents/Home/
```

现在运行jenv versions会显示

```
$ jenv versions
* system (set by /Users/bxpeng/.jenv/version)
  1.6
  1.6.0.65
  oracle64-1.6.0.65
  1.7
  1.7.0.71
  oracle64-1.7.0.71
  1.8
  1.8.0.25
  oracle64-1.8.0.25
```

如果希望看着整洁, 可以使用jenv remove可以从jenv中去掉不需要显示(不会删除jdk)的Java版本

```
$ jenv remove 1.6.0.65
JDK 1.6.0.65 removed
```

整理后, 再运行jenv versions会显示

```
$ jenv versions
* system (set by /Users/you/.jenv/version)
  1.6
  1.7
  1.8
```

### 切换jdk版本

#### 全局有效

```
$ jenv global 1.6
```

### 文件夹有效(本目录有效)

你可以在特定的文件夹下使用 .java-version 文件来设定 Java 的版本。当我需要在 Project 中使用 Java 6 时, 我仅仅需要把 1.6 作为内容保存在 .java-version 文件中, 当我进入该文件夹时 jenv 会自动地帮助我设定 local 的 Java 的版本。

```
$ jenv local 1.6
```

### shell有效

```
$ jenv shell 1.6
```

### 查看 java 路径

```
$ jenv which java
```

现在有了Java的多个版本, 并且可以在它们之间轻松切换。更多的使用方法可以在jenv官网的官网查询到。

## 二、linux-china 版本

官网 http://jenv.io/

jenv 是一个环境管理工具,  是 RVM 的 Java 版。

- 轻松管理 Java 版本, 如1.6, 1.7 和 1.8
- 轻松安装 Java 工具, 如Ant, Maven, Tomcat 等等.
- 轻松管理软件版本, 安装新版本, 或卸载旧版本
- 目录规范, 方便 IDE 使用
- 方便扩展
- 轻松备份你的环境
- 支持自动补全, 使用 TAB 补全命令, 软件名称或版本号

### Linux 或 Mac 安装 jenv

```
$ curl -L -s get.jenv.io | bash
```

### Install jenv on Windows

[](http://jenv.io/)

### Install Java

如果机器上面还没有 Java SDK, 请从官网下载 http://www.oracle.com/technetwork/java/javase/downloads/index.html 并安装.

Mac OSX Yosimate 以后集成 jdk8 , 下面给出 JDK6， JDK7下载地址

JDK6: http://supportdownload.apple.com/download.info.apple.com/Apple_Support_Area/Apple_Software_Updates/Mac_OS_X/downloads/031-03190.20140529.Pp3r4/JavaForOSX2014-001.dmg

JDK7: http://download.oracle.com/otn-pub/java/jdk/7u79-b15/jdk-7u79-macosx-x64.dmg

```
$ mkdir -p $HOME/.jenv/candidates/java
$ ln -s /Library/Java/JavaVirtualMachines/jdk1.7.0_51.jdk/Contents/home $HOME/.jenv/candidates/java/1.7.0_51
### osx10.10上面安装1.6以后的路径问题
ln -s /System/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home/ $HOME/.jenv/candidates/java/1.6.0
$ jenv default java 1.7.0_51
```

### 安装其他软件

1. 查看可以安装的软件:

    ```
    $ jenv all
    ```

2. 查看软件可用的版本, 如果 Maven:

    ```
    $ jenv list maven
    ```

3. 安装指定版本:

    ```
    $ jenv install maven 3.2.1
    ```

可以使用 mvn --version 检查安装情况.

### Install candidates in other ways

从本地文件夹中安装:

```
$ jenv install java 1.7.0_51 /user/local/jdk-1.7.0_51
```

Install from git repository:

```
$ jenv install spike 0.0.1 git@github.com:linux-china/groovy_scripts.git
```

### 获取更新的软件仓库信息

jenv 软件仓库信息为离线模式, 请使用下面命令更新仓库信息。

```
$ jenv repo update
```

### 更新 jenv

```
$ jenv selfupdate
```

### jenvrc support

jenvrc is jenv setup file which contains candidate and the version as following:

```
java=1.6.0_45
maven=3.0.5
```

After you enter this directory, jenv will setup environment automatically.

### jenv IntelliJ IDEA plugin

With jenv IDEA plugin, you don't need to setup Java SDK, Maven, and so on, and jenv IDEA plugin can scan jenv directory and setup the settings in IDEA automatically. Please visit http://plugins.jetbrains.com/plugin/?idea&pluginId=7229 for more information.

### 如何使用jenv快速建立Java环境？

下午要在一台空白的阿里云服务器上部署一个Java应用。使用jenv快速设置Java的环境。接下来是我的步骤

1.  执行jenv安装$curl -s get.jvmtool.mvnsearch.org | bash , 然后你选用重新登陆或者执行  source .bash_profile
2. 安装Java  $jenv install java 1.7.0_17 http://get.jvmtool.mvnsearch.org/download/java/java-1.7.0_17-x64.zip  , 然后执行
3. 安装Maven $jenv install maven 3.0.5
4. 安装Tomcat  $jenv install tomcat 7.0.39   安装完毕后执行 $jenv cd tomcat  进入tomcat安装目录编辑conf/server.xml

整个过程简单命令, 之前繁复的环境变量设置, PATH修改, 现在全部没有啦。

