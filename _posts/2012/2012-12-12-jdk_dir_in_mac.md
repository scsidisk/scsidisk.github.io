---
layout: post
title: "MAC中jdk的目录"
date: 2012-12-12
author: scsidisk
category: Java
tags: [Java, JDK, Mac]
---

在MAC系统中，jdk的安装路径与windows不同，默认目录是：/System/Libray/Frameworks/JavaVM.Framwork/。
在这个目录下有个Versions目录，里面有不同版本的jdk。

### 1.怎样设置mac中的默认java版本呢 ？

先看一下mac中，java链接到了哪里：

进入到相应的目录：cd /usr/bin

查看java链接到了哪里：ls -l java

```
localhost:bin root# ls -l java
lrwxr-xr-x  1 root  wheel  74 May 18 10:26 java -> /System/Library/Frameworks/JavaVM.framework/Versions/Current/Commands/java
```

可以看到java连接到了current版本。那么这个到底是什么版本呢？其实，mac中current只是一个快捷方式而已，是为了方便设置默认java的。

这个链接连到哪里，默认的java就是哪个。但是在mac中可以保持这个java链接不变，只是改变一下当前的java即可，下面是步骤：

1）打开Finder ： 单击桌面地步的finder图标即可。

2）Application-->Utilities-->Java-->Java Preferences

3) 由第二步可以打开“Java Preferences”对话框，选中“General”tab。在下面的“Java Application Runtime Settings”区把需要的java版本拖动到最顶端即可。

最顶端的java就是当前（current）java，这样在改变默认java版本时就不用在/usr/bin下重新设置java链接，而是直接在这里把需要的java拖到最上面就行。

### 2.MAC中的javahome设置

在windows中，javahome的值只是取到版本号的目录即可，但是在mac中有稍微的不同，要去到版本号目录下的Home目录，如：

```
JAVA_HOME=/System/Library/Frameworks/JavaVM.framework/Versions/1.6.0/Home
```

### 3.在MAC中设置JAVA_HOME环境变量

环境变量要再etc目录下的profile文件中配置，这样才是永久的配置。

```
cd /etc
vi profile
```

输入如下内容：

```bash
JAVA_HOME=/System/Library/Frameworks/JavaVM.framework/Versions/1.6.0/Home
export JAVA_HOME
```

保存。然后重启或者注销，使环境变量的配置起作用。

这样javahome的环境便令配置好了。

同样的道理，我们可以在profile这个文件中进行PATH,CALSSPATH等环境变量的配置。