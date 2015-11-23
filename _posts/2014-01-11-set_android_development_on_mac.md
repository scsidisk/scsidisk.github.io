---
layout: post
title: Mac配置android开发环境
date: 2014-01-11
author: scsidisk
category: 移动开发
tags: [Android, Java]
---

### Eclipse的下载

到网站：http://www.eclipse.org/downloads 上，由于我们是用Java开发的所以步骤如下：

- Eclipse Standard 4.3.1, 196 MB 右上角选择“Mac OS X（Cocoa）”
- 然后点击右边的“Mac OS X 64bit”

### 安装ADT

ADT是Android应用程序的开发环境

在线安装，本来还有个离线安装的，但是我试图去下载这个离线安装包但是没有找到下载的地方，所以这里我主要介绍如何进行在线安装。

- 点击菜单中的Help ——> Install New Software⋯ ;
- “Work with”右边输入https://dl-ssl.google.com/android/eclipse看到pending出来一个“Developer Tools”，勾选上，然后一路的Next下去就可以安装完成。

### 设定ADT

在菜单栏Refactor中如果能看到Android的标签表示ADT安装成功。

在菜单栏window中应该可以看到Android SDK Manager

#### 下载Android SDK

点击展开 USE AN EXISTING IDE, 点击 Download the SDK Tools for Mac 下载

#### 安装Android SDK

刚下载的Mac版的SDK文件是：android-sdk_r22.3-macosx.zip，将其解压出来，放到你的开发程序目录.

然后在菜单栏Eclipse —> Preferences（偏好设置）,会弹出一个Preferences对话框，选Android，然后在SDK Loaction中填入刚下载的SDK的路径或者点击右边的Browser选择。

在eclipse->window->Android SDK Manager点击后出现新窗口,选择自己要安装的包

我安装的包如下

- Tools/Android SDK Tools
- Tools/Android Platform-tools
- Tools/Android Build-tools
- Android 4.1.2 (API 16)

#### 生成模拟器

菜单栏Window —> Android SDK and AVD Manger 会弹出对话框，然后在对话框中选择new开始按自己的需求新建模拟器，至此就大功告成了！

### 在Mac环境下配置Android环境变量

在解决mac中为图形界面程序设置环境变量问题中提到在maven中的配置，看了一下，android sdk tools路径的配置没记过。这里补充一下。

编辑或者创建文件：

```
vim ~/.bash_profile
```

在里面把tools路径加上：

```
export PATH=${PATH}:/Developer/android-sdk-mac_86/tools
```

然后，执行命令，让配置生效：

```
source ~/.bash_profile
```

测试一下：

```
adb devices
```