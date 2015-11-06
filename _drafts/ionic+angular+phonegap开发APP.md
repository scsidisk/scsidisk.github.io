

## 准备环境

$ npm install -g cordova ionic
$ npm install -g gulp
$ npm install -g ios-sim

## 生成项目

$ ionic start myApp tabs
$ cd myApp/
$ ionic setup sass
$ ionic platform add android
$ ionic serve // 打开浏览器进行开发
$ ionic build android
$ ionic emulate android
$ ionic run android
$ ionic package android
$ cordova build --release android //未签名

添加 ios 平台

$ ionic platform add ios
$ ionic build ios
$ ionic emulate ios


## 创建项目以后的提示

Your Ionic project is ready to go! Some quick tips:

 * cd into your project: $ cd myApp

 * Setup this project to use Sass: ionic setup sass

 * Develop in the browser with live reload: ionic serve

 * Add a platform (ios or Android): ionic platform add ios [android]
   Note: iOS development requires OS X currently
   See the Android Platform Guide for full Android installation instructions:
   https://cordova.apache.org/docs/en/edge/guide_platforms_android_index.md.html

 * Build your app: ionic build <PLATFORM>

 * Simulate your app: ionic emulate <PLATFORM>

 * Run your app on a device: ionic run <PLATFORM>

 * Package an app using Ionic package service: ionic package <MODE> <PLATFORM>

For more help use ionic --help or visit the Ionic docs: http://ionicframework.com/docs

## 安装 angular-chart

npm install -g bower
bower install angular-chart.js --save
bower install d3 --save

会安装到 www/lib/js

## 生成图标和启动画面

- 在项目的根目录下创建 resources 文件夹。
- 在 resources 中放入图标文件 icon.png，最小192x192px，不带圆角。
- 在 resources 中放入启动屏幕文件 splash.png，最小2208x2208px，中间区域1200x1200px）(可以是png、psd、ai)
- 在cmd中进入项目所在文件夹执行：ionic resources ， 自动生成不同分辨率的图片，并在config.xml中添加相应内容。

也可分开执行：

```
ionic resources --icon
ionic resources --splash
```

注意：执行以上命令时需在线！


Getting started with Ionic

https://coderwall.com/p/vvkyra/getting-started-with-ionic

I have recently started using Ionic for developing mobile apps, and it is awesome...

You have Node and npm on your machine right...

npm install -g cordova ionic
Okay now create your app folder, cd into it, then start your app from a template

ionic start myApp tabs
cd myApp
ionic platform add ios
ionic build ios
ionic emulate ios
note: You need a mac to build and publish an IOS app, but you can develop on Windows, or Linux, then once you are ready, jump onto a mac for the build process

Another way to quickly get up and running for testing on your local web server, is to fork or download an existing seed app to play around with. Here is a kitchen sink example. The Ionic book is also a great guide to building your first app with Ionic.

Am I using PhoneGap or Cordova?

PhoneGap was originally developed by Nitobi with the following goal: "To build the bulk of a mobile app experience with pure web technologies like HTML5, CSS, and Javascript, but still be able to call into native code when necessary."

In 2011 Adobe purchased Nitobi and the rights to the PhoneGap brand. The code was donated to the Apache Software Foundation under the name Cordova.

Ionic UI framework

Ionic is a great framework with a lot of the UI elements we need for our mobile apps. It is really more of a competitor to Twitter Bootstrap. Ionic and Bootstrap are the UI elements, layouts, buttons etc… which are used in the HTML5 mobile app.

Let's see some UI elements then

Just check the documentation for lovely Ionic CSS, JavaScript and HTML5 elements. They really are easy to use for any web developer and look extremely pretty.

PhoneGap/Cordova API

PhoneGap or Cordova are the native "bridge" that our HTML5 app will use to give it access to all the features of a native app for the Apple and Google Play stores.

Build process

The Build system is the process of compiling these apps into an .ipa file for IOS and .apk file for Android. We can use PhoneGap build for this, or Cordova build. PhoneGap is $9.99 per month for private repositories, Cordova is free and as simple as the commands below. The cordova and ionic build commands are one and the same in terms of functionality so just pick one of them.

cordova build --release android
cordova build --release ios

ionic build ios
ionic build ios
Publish your app to Google Play Store

First remove any development plugins you have in your app.

cordova plugin rm org.apache.cordova.console
Make sure your manifest file has debug set to false

<application android:debuggable="false" android:hardwareAccelerated="true" android:icon="@drawable/icon" android:label="@string/app_name">    
Build the app using the ionic or cordova build command

cordova build --release android
We can now find our unsigned APK file in platforms/android/bin. Let's generate our private key using the keytool command that comes with the JDK

keytool -genkey -v -keystore my-release-key.keystore -alias alias_name -keyalg RSA -keysize 2048 -validity 10000
Now your .keystore file is created in the current directory, sign the unsigned APK with the key. Run the jarsigner tool which is also included in the JDK

jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore my-release-key.keystore HelloWorld-release-unsigned.apk alias_name
We have a signed apk, and finally need to run the zip align tool to optimize the APK:

zipalign -v 4 HelloWorld-release-unsigned.apk HelloWorld.apk
Now that we have our release APK ready for the Google Play Store, we can create a Play Store listing and upload our APK. Pay your $25 to sign up as a Google Play Store App developer then you are ready to upload your app by simply clicking "Publish an Android App on Google Play".

Publish your app to Apple Store

You are on a mac for this part I hope :)

First remove any development plugins you have in your app.

cordova plugin rm org.apache.cordova.console
Join the iOS Developer Program

Other than using PhoneGap to develop ipas for you, the only way to build IPAs for testing on your devices is by joining Apple’s iOS developer program

Jump in to the iOS Developer Center and register for free. Being a registered Apple developer gives you access to a lot of information, but to be able to send apps to the App Store or to generate IPAs from XCode you will need to enroll in Apple’s iOS developer program. This is the part that costs you US$99 per year.

In your Developer Member Center click the join today link. You will be asked to enrol as a company or individual, most will be enrolling as an individual but if you need to enrol as a company you will need some documentation from your employer.

Once you proceed you will be asked to Sign in with your Apple ID for billing. You will be asked to select your Program, most likely the IOS Developer program. Add it to Cart and make your payment.

Build your iOS app and open in XCode

After submitting and paying for your iOS Developer registration, you’ll need to wait around a day for Apple to process your order. For me it activated overnight in about 6 hours.

If you don't already have it, download XCode. You need this to deploy your app. Also log in to your Apple Developer Center

Jumping back to you Ionic app, you have no doubt developed, Once your app is ready, run the build ios command.

ionic platform add ios
ionic build ios
This will generate an Xcode project inside of platforms/ios which you can open with Xcode and sign your app for publishing.

Deploying the app to your iPad or iPhone for testing

The app is built and ready to be pushed out as an IPA.

You have to find the IOS application, and launch it in XCode. It has been generated for you in /platforms/ios so if your app was IonicTest, the XCode project will be IonicTest.xcodeproj in IonicTest/platforms/ios/

Open the file in XCode so you can deploy your IPA.

Create Provisioning Profile

Now to deploy your IPA to a device you will need to Create a Provisioning Profile. In your Developer account, click on Provisioning Profile and set one up.

Verify that the Code Signing section in your app in XCode is set to your provisioning profile name. In the Identity section in the General tab it should have your name in the Team field.

Set your device

In the XCode File menu, select Product > Destination and select "IOS device". You will now need to select a Device ID that you are deploying to,

Jump in to Devices in Apple Developer Centre and add your device UDID.

The easy way to find your devices UDID is to follow this guide but it is pretty straight forward. Now add your device, or if you want to deploy to multiple devices you can add some additional ones.

Exporting to an IPA

The easiest way to generate an IPA is to archive your applications and share it from the Xcode Organizer.

Build your app, Click Product > Build then Product > Archive to open up XCode Organizer. Click on Distribute. This is where you select to either publish your app to the Apple App Store or just export the IPA to be installed on your device. Select "Save for Enterprise or Ad Hoc Deployment". Click Next then select your provisioning profile.

Now finally, Click Export to get your IPA file.

You have finally exported the IPA to your file system. Select a folder where you will be keeping all of your versions of IPA files and Click Save

Install your IPA on a device

Now you have your IPA file, you need to push this IPA to your device. An easy way to do this is to Connect your Device to your MacBook and open iTunes. Select the Apps tab, then from the file menu select File > Add to Library to add your IPA file. It will ask you to Install the app, it will now be running as an app on your iPhone or iPad.

Use Test Flight for testing on multiple devices

If you now want to push new versions of your app to yours or multiple devices quickly, you should Sign up to TestFlight. I will most likely be setting up a tutorial on TestFlight but it is nice and easy to use. As long as the devices have the TestFlight app installed and you add their Device ID to your devices you should be able to push out to multiple devices with one easy step.

Some links if you got into trouble deploying your IPA:

http://cordova.apache.org/docs/en/3.3.0/guide_platforms_ios_index.md.html

http://www.raywenderlich.com/8003/how-to-submit-your-app-to-apple-from-no-account-to-app-store-part-1

http://help.testflightapp.com/customer/portal/articles/1333914-how-to-create-an-ipa-xcode-5-