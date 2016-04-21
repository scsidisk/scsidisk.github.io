---
layout: post
title: 在 Mac 上使用 NativeScript
date: 2015-06-15 12:55:55
author: scsidisk
category: 移动开发
tags: [Mac, Android]
---

NativeScript 是 Telerik 发布的用于创建安卓、iOS和Windows Universal跨平台原生应用的框架。

下面说明在 Mac OSX 上面的实践过程。

### 1. 环境依赖

- OS X Mavericks or later
- Node.js 0.10.26 or a later stable official release except Node.js 0.10.34
- (Optional) Homebrew to simplify the installation of dependencies
- For iOS development
	- Latest Xcode
	- Command-line tools for Xcode
- For Android development
	- JDK 8 or a later stable official release
	- Apache Ant 1.8 or later
	- Android SDK 19 or later
	- (Optional) Genymotion to expand your testing options

### 2. 安装

```
$ brew install node ant android-sdk
$ npm i -g nativescript
$ android sdk ## 选择 API22和tools然后下载
```

下面是我安装的包，如果你的网速比较慢，可以在进行精简。

```
## tools
	Android SDK Platform-tools, revision 22
	Android SDK Build-tools, revision 22.0.1

## Android 4.2.2(API 17) 全部

## Android 5.1.1(API 22) 除了TV,Wear,Documentation以外的包
	SDK Platform Android 5.1.1, API 22, revision 2
	Samples for SDK API 22, revision 6
	Sources for Android SDK, API 22, revision 1
	Android Support Library, revision 22.2
	Android SDK Tools, revision 24.3.2
	Google APIs, Android API 22, revision 1
	##可选
	ARM EABI v7a System Image, Android API 22, revision 1
	Intel x86 Atom_64 System Image, Android API 22, revision 1
	Intel x86 Atom System Image, Android API 22, revision 1
	Google APIs ARM EABI v7a System Image, Google Inc. API 22, revision 1
	Google APIs Intel x86 Atom_64 System Image, Google Inc. API 22, revision 1
	Google APIs Intel x86 Atom System Image, Google Inc. API 22, revision 1
```

### 3. 设置手机

如果你想在手机上面的进行测试，需要设置手机 USB Debug 模式。 如果只是运行模拟器，跳过此部分。

以小米4为例：

设置 -> 关于手机 -> MIUI版本 -> 快速连击4次 -> 手机进入USB调试模式

### 4. 创建项目

```
$ tns create hello-world
$ cd hello-world
$ tns platform add ios
$ tns platform add android
```

### 测试运行

```
$ tns run android --emulator
如果是手机上面调试：
$ tns run android
```

### FAQ

1. ant jar error: Execute failed: java.io.IOException: Cannot run program…${aapt}": error=2, No such file or directory

Have you updated the Android SDK tools to 24.3.2? This seems to have caused the issue. Add following 4 lines to android-sdk-path/tools/ant/build.xml starting line 484 and hopefully it should solve.

```
<property name="aidl" location="${android.build.tools.dir}/aidl${exe}" />
<property name="aapt" location="${android.build.tools.dir}/aapt${exe}" />
<property name="dx" location="${android.build.tools.dir}/dx${bat}" />
<property name="zipalign" location="${android.build.tools.dir}/zipalign${exe}" />
```

其他内容参见 [官方文档](http://docs.nativescript.org/)