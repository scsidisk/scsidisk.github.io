---
layout: post
title: " React Native for Android on mac"
date:   2015-12-16
categories: ReactNative
tags: ReactNative, Android
---


开发环境
-------

- OS X
- Android 4.4.4
- MIUI7 开发版
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
* `targetSdkVersion` 在编译程序是指定一个API Level，我一般配置为跟 compileSdkVersion一致。

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

### ANDROID_HOME 设置不正确

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

### 找不到 'android-23'

sdk 问题，重新安装上面的sdk相应的项目，正确设置 `ANDROID_HOME`, 可以解决。或者使用下面的方法(未验证)。

    1. 處理方式，更新 android sdk
    2. 執行 `android update sdk --no-ui`
    3. 執行 `android update sdk -u -a`

### 找不到 device 裝置

    1. 使用模拟器，安裝 `genymotion`
    2. 使用实机，数据线连接手机
    3. 执行 `adb devices` 查看设备列表

### `adb reverse` 调试

如果不是 `Android 5.0+ (API 21)` ，那么就没办法通过 `adb reverse` 进行调试，需要通过 WiFi 来连接上你的开发者服务器

### app 名称修改

    安卓版本的应用名在这里：

    $ vim android/app/src/main/res/values/strings.xml
    <string name="app_name">MyProject</string>

MyProject改成你需要的名字就好了。

图标在 `android\app\src\main\res` 文件夹下每个 mipmap 开头的文件夹下有一个不同尺寸的版本，可以自行输出，也可以使用 Android Studio 批量修改。

修改完成后重新运行或打包即可。

### React Native for Android 发布独立安装包

React Native Android 开发的话，必须启动个 JS Server，然后要让手机连接这个 Server。

如果要发布一个 React Native 写的 Android 应用，不可能要别人来连接这个 JS Server。可不可以不要连接这个 Server 就能运行呢？在网上找了一圈，发现资料很少，官方文档上也没有说支持。这篇文章就来讨论一种实现方案。

第一次打开 React Native 应用的时候，会连接 JS Server 下载一个 ReactNativeDevBundle.js 文件，然后放到应用数据的 files 目录下，就能运行这个 JS 文件了。到这里我们也就找到解决方案了。

只要我们把这个 ReactNativeDevBundle.js 文件提前打包到 APK 中，然后 APK 运行的时候，把这个文件释放到 files 目录中。

我们可以把 ReactNativeDevBundle.js 先保存下来，放在 Android 工程的 assets 目录中，这会自动打包到 APK 中。在 APP 第一次运行的时候，把文件拷贝到目的位置，代码如下：


    public class MainActivity extends Activity {

        private static final String JSBUNDLE_FILE = "ReactNativeDevBundle.js";

        private static void copyFile(InputStream in, OutputStream out) throws IOException {
            byte[] buffer = new byte[1024];
            int read;
            while((read = in.read(buffer)) != -1){
                out.write(buffer, 0, read);
            }
        }

        private void prepareJSBundle() {
            File targetFile = new File(getFilesDir(), JSBUNDLE_FILE);
            if (!targetFile.exists()) {
                try {
                    OutputStream out = new FileOutputStream(targetFile);
                    InputStream in = getAssets().open(JSBUNDLE_FILE);

                    copyFile(in, out);
                    out.close();
                    in.close();
                } catch (FileNotFoundException e) {
                    e.printStackTrace();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            prepareJSBundle();
            ...
        }
    }

使用实例可以参考这里：[MainActivity.java](https://github.com/race604/ZhiHuDaily-React-Native/blob/master/android%2Fapp%2Fsrc%2Fmain%2Fjava%2Fcom%2Frace604%2Fzhihu%2Fdaily%2FMainActivity.java)。

在 build.gradle 中添加一个 task，让它每次打包的是自动访问 http://localhost:8081/index.android.bundle?platform=android 下载文件到 app/src/main/assets/ 目录中，脚本如下：

    final def TARGET_BUNDLE_DIR = 'app/src/main/assets/'
    final def TARGET_BUNDLE_FILE = 'ReactNativeDevBundle.js'
    final def DOWNLOAD_URL = 'http://localhost:8081/index.android.bundle?platform=android'

    task downloadJSBundle << {
        def dir = new File(TARGET_BUNDLE_DIR)
        if (!dir.exists()) {
            dir.mkdirs()
        }
        def f = new File(TARGET_BUNDLE_DIR + TARGET_BUNDLE_FILE)
        if (f.exists()) {
            f.delete()
        }
        new URL(DOWNLOAD_URL).withInputStream{ i -> f.withOutputStream{ it << i }}
    }
    // 保证每次编译之前，先下载 JS 文件
    preBuild.dependsOn downloadJSBundle

这样，每次打包的时候，都会先帮我们下载好 JS 文件到指定位置了。当然，打包的时候，你要保证你的 JS Server 是开着的。完整代码可以参考：[build.gradle](https://github.com/race604/ZhiHuDaily-React-Native/blob/master/android%2Fapp%2Fbuild.gradle)。

到这里，我们就实现了一个可行的方案了，可以独立发布 APK 了。

我这里只是一个简陋的解决方案，这里有些问题需要改进。首先，我们的 JS 文件都是明文的，基本上就是你的源代码，用在生产环境的话，做混淆是必须的。相信官方很快也会出标准的解决方案，毕竟 iOS 已经支持了。另外，如果要做在线更新的话，需要保证你更新 JS 的服务器的安全，因为这些 JS 代码可以直接运行到用户手机上。

参考
----

- https://facebook.github.io/react-native/docs/getting-started.html
- http://www.race604.com/react-native-for-android-start/
- http://react-native.cn/docs/getting-started.html
- https://github.com/ggchxx/React-Native-Android-Config
