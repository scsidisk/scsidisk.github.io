 ionic 在Mac os上环境搭建及开发过程遇到的问题
2015-03-20 12:27:58
标签：ionic
原创作品，允许转载，转载时请务必以超链接形式标明文章 原始出处 、作者信息和本声明。否则将追究法律责任。http://demidroid.blog.51cto.com/2954092/1622505
Ionic 介绍
Ionic是一个前端的框架，结合html5，css3和javascript做出类似native app的移动应用，Ionic框架很先进，js部分是基于angular js框架，大量使用css3，版本升级基于bower，Ionic关注应用的外观、体验和UI用户界面交互，这个框架除了带有SASS服务和各种各样的AngularJS拓展（可选）之外，还有大量的组件。
在mac系统下的ionic环境搭建
首先确保已经安装了node.js,可以使用 node -v这个命令来验证，如果你还没有安装可以到下面这个网址去下载安装 http://nodejs.org/

在mac 系统下安装cordova和ionic的命令
sudo npm install -g cordova ionic
如果您已经安装，要确保已经更新到最新的版本，使用下面的命令
sudo npm update -g cordova ionic
使用ionic创建一个应用名为 myApp以tabbar为基础(除了tabs，还包括slidemenu等)
        $ ionic start myApp tabs
   5.设置ionic的编译的平台
$ ionic login
$ cd myApp
$ ionic platform add android
$ ionic build android
$ ionic run android
搭建过程中遇到的问题
ionic start myApp
Creating Ionic app in folder /Users/boteng/myApp based on tabs project

Downloading: https://github.com/driftyco/ionic-app-base/archive/master.zip
  Error fetching: https://github.com/driftyco/ionic-app-base/archive/master.zip Error: connect ETIMEDOUT
  Error: Unable to initalize app: Error: connect ETIMEDOUT
 需要设置代理，翻墙到外网
PROXY=http://127.0.0.1:8087 ionic start myApp

学习资料 ionic 中文指南
https://github.com/ychow/ionic-guide
