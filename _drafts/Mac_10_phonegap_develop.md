# Mac 10.10 下安装配置 phonegap 开发环境

### 1. 安装 Node

    $ brew install node

### 安装 Ant

### 安装 cordova

    $ npm install -g cordova

### 安装 phonegap

    $ npm install -g phonegap

### 创建项目

再当前目录创建hello项目

创建phonegap项目名字暂时就叫helloworld吧，cordova即phonegap是由于adobe收藏原phonegap后另外取的名字而已

hello即项目名，com.example.hello为(id)命名空间, HelloWorld即APP名称

    $ cordova create hello com.example.hello HelloWorld

### 添加要编译的平台

即项目开发完后要编译出哪些平台的APP，如ios平台，android平台，Blackberry平台等，详情支持哪些平台请看这里

http://docs.phonegap.com/en/edge/guide_platforms_index.md.html#Platform%20Guides

- 进入hello项目目录

    $ cd hello

#### 1. 添加ios平台

    $ cordova platform add ios
    $ cordova platform add android

- 创建ios平台项目

    $ cordova build ios
    $ cordova build android

双击 platforms/ios/HelloWorld.xcodeproj 这个文件就可以在 XCode 中打开这个项目进行测试了

接下来就可以在xcode中正常的编译、输出、发布到appstore上了，前提是您得有开发者帐号哈。添加ios平台是如此的简单


#### 2. 添加android平台

添加 ant

    $ brew install ant

安装并正确配置android SDK

取得最新的android SDK并添加至全局环境中(可能需要翻一下墙) http://developer.android.com/sdk/index.html

下载后解压并放到某个目录下，我是放在了家目录下的Development（自建）目录下，查命名为 android-sdk-macosx

由于下载的最新SDK只是个基本环境，双击 tools 目录下的 android 程序 android SDK manager 来下载更新安装你需要的 N 个 android 版本可能会花点时间哟， 勾选你需要使用的版本，我是选了4.4.2的.

更新完SDK后得配置Android Virtual Device Manager即传说中的AVD也就是android虚拟机

双击tools目录下的monitor程序，monitor然后点击window菜单下的monitor打开AVD管理器

点击 Create 生成自己的设备，参数可以参考自己的手机，或网上的参数

将android SDK目录添加到全局环境中

    $ vim ~/.bash_profile

打开文件后输入这两行

```
export PATH=/Applications/Develop/android-sdk-macosx/platform-tools:$PATH
export PATH=/Applications/Develop/android-sdk-macosx/tools:$PATH
export ANDROID_HOME=/Applications/Develop/android-sdk-macosx/tools/:/Applications/Develop/android-sdk-macosx/platform-tools/
```

具体的目录需要更改为你放置android SDK的正确目录

保存文件

并在Terminal内输入

    $ source ~/.bash_profile

此命令是刚刚的配置命令以即刻生效

好了，android的所有配置完成了。

继续运行命令

    $ cordova platform add android

添加android平台就可以成功了

使用eclipse就可以直接导入项目

在Terminal中输入

    $ cordova build android

即可编译出apk文件在hello/platforms/android/bin目录下可以找到

在模拟器中运行

    $ cordova emulate android

也可以手动安装apk至android手机

至此ios与 android平台的配置都已经OK了。Android的配置真是蛋疼啊。

转载：博客园偷饭猫