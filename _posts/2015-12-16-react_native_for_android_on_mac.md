---
layout: post
title:  React Native for Android on mac
date:   2015-12-16
categories: ReactNative
tags: [ReactNative, Android]
---


开发环境
-------

- OS X
- Homebrew 命令, 参见 [](http://scsidisk.github.io/2015/11/use_homebrew_on_mac/)
- Node.js 4或更高，如果用node5，最好使用npm2
    - `brew install node`
    - `npm install -g npm@2`
- watchman 推荐安装。`brew install watchman`
- flow 可选。`brew install flow`

快速开始
-------

    $ npm install -g react-native-cli
    $ react-native init AwesomeProject

安装 Android-SDK
----------------

    $ brew install android-sdk

设置环境变量和路径，添加下面代码到 `~/.bash_profile`

    # If you installed the SDK via Homebrew, otherwise ~/Library/Android/sdk
    export ANDROID_HOME=/usr/local/opt/android-sdk
    export PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools

重载配置文件

    $ . ~/.bash_profile
    $ android

在弹出的界面中添加下面的项目

    Tools
        Android SDK Tools, revision 24.4.1
        Android SDK Platform-tools, revision 23.1
        Android SDK Build-tools, revision 23.0.1
        Android SDK Build-tools, revision 23.0.2
    Android 6.0 (API 23)
        SDK Platform, API 23, revision 2
        Intel x86 Atom System Image, Android API 23, revision 6
        Intel x86 Atom_64 System Image, Android API 23, revision 6
    Extras
        Android Support Repository, revision 25
        Android Support Library, revision 23.1.1

连接手机
-------

### 1. 手机设置为 `USB 调试模式`

选择手机上的 `设置`, `关于手机`, 在 `MIUI版本` 上面多点击几次，提示 `您已处于开发站模式即可`。

选择手机上的 `设置`, `其他高级设置`, `开发者选项`, 开启 `USB调试`。

### 2. 连接电脑

手机通过数据线连接到电脑

安装应用
-------

### 1. 检查配置

Gradle中配置的API Level 必须在sdk已安装的版本一致

* `compileSdkVersion` SDK的版本号，也就是API Level，例如API-19、API-20、API-21等等
* `buildToolsVersion` 构建工具的版本，包括打包工具aapt、dx等等。位于 `sdk_path/build-tools/XX.XX.XX`
*  `targetSdkVersion` 在编译程序是指定一个API Level，我一般配置为跟 compileSdkVersion一致。

### 2. 创建assets目录

    $ cd AwesomeProject
    $ mkdir android/app/src/main/assets
    # 在新的终端里面执行:
    $ react-native start
    # 上面的命令执行完成以后，保持页面不关闭
    # 执行下面命令复制资源文件
    # 如果assets目录中不存在该文件，则实机打包的apk在执行时显示空白
    $ curl -k 'http://localhost:8081/index.android.bundle' > android/app/src/main/assets/index.android.bundle

### 2. 配置签名

未签名应用非 `root` 不允许安装, 如果已经 `root` 可以忽略。

添加gradle的android keystore配置，在build.gradle文件中，具体配置如下

    //签名
    signingConfigs{
        release {
            storeFile file("/Volumes/Android.KeyStore/AndroidDebug.keystore")
            storePassword "密码"
            keyAlias "keyAlias的名字"
            keyPassword "密码"
        }
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            signingConfig signingConfigs.release //添加这句话引用签名配置
        }
    }

### 4. 安装应用到手机

执行下面命令。

    $ react-native run-android

也可以生成apk文件复制到手机(不推荐)

    $ cd android/
    $ gradle assembleRelease

打包后的文件在 android/app/build/outputs/apk目录中,
如果打包碰到问题可以先执行 gradle clean 清理一下,
将apk复制到手机中安装运行

设置应用权限
-----------

`安全中心`, `权限管理`, `应用权限管理`, 找到 `AwesomeProject`, 允许 `显示悬浮层`。

连接开发电脑
-----------

查看手机和开发电脑要在同一个网段里面。

打开应用的情况下, 长按 `MENU` 菜单键, 在弹出的菜单中选择 `Dev Setting`, 点击 `Debug server host & port for device`, 添加开发机器的 IP:8081。

开发电脑需要使用 `react-native start` 开启服务端。

更新 JS 或 热加载
----------------

打开应用的情况下, 长按 `MENU` 菜单键, 弹出的菜单.

- Reload JS 重新加载js
- Enable Live Reload 热加载

FAQ
---

### 静态资源问题

在打包文件时需要注意curl 下载的Bundle文件要与Activity中配置的一致，而不是统一的index.android.bundle

例如 Movies项目中，在Activity的OnCreate中

    mReactInstanceManager = ReactInstanceManager.builder()
        .setApplication(getApplication())
        .setBundleAssetName("[MoviesApp.android.bundle]()")
        .setJSMainModuleName("MoviesApp.android")
        .addPackage(new MainReactPackage())
        .setUseDeveloperSupport(true)
        .setInitialLifecycleState(LifecycleState.RESUMED)
        .build();

这个 `BundleAssetName` 的名字为 `MoviesApp.android.bundle`，所以在添加 bundle 到 assets 目录时，需要改为

    $ curl -k 'http://localhost:8081/MoviesApp.android.bundle?platform=android' > android/app/src/main/assets/MoviesApp.android.bundle

### ANDROID_HOME设置不正确

> **A problem occurred configuring project ':app'.
    failed to find target with hash string 'android-23' in: /Users/zbmobi/DevelopTools/Android/sdk**

### compiletargetSdkVersion 版本号不一致

> ***A problem occurred configuring project ':app'.
 failed to find Build Tools revision 23.0.1***

解决办法: 查看build-tools目录，修改compileSdkVersion和targetSdkVersion的对应版本号

### buildToolsVersion 版本号不一致

> ***A problem occurred configuring project ':app'. Could not resolve all dependencies for configuration ':app:_debugCompile'. Could not find com.android.support:appcompat-v7:23.0.0.***

解决办法: 查看build-tools目录，然后修改为已存在的版本号

### npm安装包的时候，出现 `libgcc_s` 错误

    $ sudo ln -s /usr/lib/libSystem.B.dylib /usr/local/lib/libgcc_s.10.5.dylib
    $ sudo ln -s /usr/lib/libSystem.B.dylib /usr/local/lib/libgcc_s.10.4.dylib

参考
----

- https://facebook.github.io/react-native/docs/getting-started.html
- http://www.race604.com/react-native-for-android-start/
- http://react-native.cn/docs/getting-started.html
- https://github.com/ggchxx/React-Native-Android-Config
