Ionic Framwork 安装开发笔记

最近由于工作的关系，接触了一些Hybrid App开发，总体来讲使用Hybrid的方式来开发一款手机APP应用，还是有蛮多优点，原因也很简单，Hybrid区别于传统Native开发，运行于手机的WebView浏览器内核，外加不能再火的大HTML5，Web工程师是可以无缝接入的，跨平台，一套解决方案搞定Android, IOS, Web端。而且与传统native开发比较大的区别就是Hybrid的资源都是在本地的，可以一次性载入，然后直接在客户端进行界面切换，节省了加载成本。
But! Hybrid开发也是一把双刃剑，Android与IOS各个版本之间的交互体验有明显差别(特别是Android的UI性能...需要花更多时间进行UI优化)，不能像Natvie开发那样有效的管理内存，支配CPU资源分配等等。
一张图的说明：嘛, 之后便接触了Ionic，其实市面上的框架也不少，Famo.us，FrameWork 7，onsen 等，用Ionic主要是看重它的UI体验，并且使用angluarjs作为前端开发框架，嗯，正好拿来练练手。
前置需求:
安装 Node.js
安装 Python (2.7.3版本即可)
安装 最新Cordova command-line tools (前身是PhoneGap，为毛换名字, 我也不知道...)
安装Ionic:
$ npm install -g cordova ionic
新建项目：
$ ionic start firstApp blank
$ cd firstApp
$ ionic platform add android
$ ionic build android安装Sass:
$ ionic setup sass或手动安装：
$ npm install gulp-sass
$ npm install gulp-minify-cssPC端测试(浏览器)：
$ ionic serve --port 8100手机模拟测试：
本人用的是Windows, 所以首先参照官网教程 Installing Cordova and the Android SDK on Windows 7 and 8配置环境。
如果是IOS开发，则可以参考教程 Introduction to Ionic for iOS
这里官方推荐了一款android emulator工具 genymotion，用于在PC端测试，还是非常好用的。普通用户就选择            personal的方式（免费的），其他类型都是要收费的。当然，也可以用Android Studio中的模拟器，凭个人喜好把~
以上安装完毕以后, 运行：
$ ionic build android打开android emulator, 运行：
$ ionic run android运行app截图：项目实例：
官方blog中推荐了几款基于Ionic开发的app产品，有兴趣的话可以下载玩玩~可能需要墙一下~viceversa
PacificaInoic 官方论坛也还是比较活跃的, 技术团队一直在更新Ionic, 而且最近也集成了CrossWalk,替换了原先的WebView, 使得android的UI渲染性能得到一定的提升,虽然和Cordova还有一定的兼容性问题，不过我相信之后的版本会越来越好的。
