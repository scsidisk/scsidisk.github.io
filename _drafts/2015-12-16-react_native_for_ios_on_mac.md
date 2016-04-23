---
layout: post
title: " React Native for IOS on mac"
date:   2015-12-16
categories: ReactNative
tags: ReactNative, IOS
---


开发环境
-------

- OS X
- Xcode `10.7 以上, 重要`
- Homebrew 命令, 参见 [](http://scsidisk.github.io/2015/11/use_homebrew_on_mac/)
- Node.js 4或更高，如果用node5，最好使用npm2
    - `brew install node`
    - `npm install -g npm@2`
- watchman 推荐安装。`brew install watchman`
- flow 可选。`brew install flow`

> Xcode `10.7 以上, 重要`

快速开始
-------

    $ npm install -g react-native-cli
    $ react-native init AwesomeProject
    $ cd AwesomeProject
    $ react-native start
    $ open ios/AwesomeProject.xcodeproj # 使用xcode打开，如果不是，双击文件打开

连接手机
-------

首先，你要让调试用电脑和你的手机必须处于相同的 WiFi 网络中下

打开 iOS 项目的 `AppDelegate.m` 文件

更改 `jsCodeLocation` 中的 `localhost` 改成你电脑的局域网IP地址

在 Xcode 中，选择你的手机作为目标设备，Run 即可

可以通过晃动设备来打开开发菜单(重载、调试等)

React Native常用组件
-------------------

- react-native-store //本地数据库
- react-native-swiper //幻灯片
- react-timer-mixin //定时器
- react-native-router //路由
- react-native-side-menu //侧边栏
- react-native-parallax-view //滚动拉升图片
- react-native-segmented-view //tab下划线缓动效果
- react-native-refresher //listview下拉刷新
- react-native-progress-bar //进度条
- react-native-modalbox //弹框
- react-thesidebar //侧边栏效果
- react-native-cookies //Cookie管理器
- react-native-lightbox //图片灯箱
- react-native-slider //滑块选择数字
- react-native-addressbook-ios //访问通讯录
- react-native-qrcode-reader //扫描二维码

FAQ
---

### `_RCTSetLogFunction` 错误

    Undefined symbols for architecture arm64:
      "_RCTSetLogFunction", referenced from:
          -[PropertyFinderTests testRendersWelcomeScreen] in PropertyFinderTests.o
    ld: symbol(s) not found for architecture arm64
    clang: error: linker command failed with exit code 1 (use -v to see invocation)

在 `Build Setting` 中设置 `Dead Code Stripping` 为 No 就可以解决了


参考
----

* https://facebook.github.io/react-native/docs/getting-started.html
* http://react-native.cn/docs/getting-started.html
* http://react-native.cn/docs/troubleshooting.html
* http://blog.yourtion.com/run-react-native-on-device.html
