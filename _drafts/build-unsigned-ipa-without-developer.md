# Build unsigned .ipa without Developer Account on Xcode 5

Wednesday, January 29, 2014

Xcode 6 已经可以在编译设置进行配置。不用修改 Xcode 的配置文件。

## To Disable Code Signing:

### Step 1:

GoTo `/Applications` then right click `Xcode.app` and click `Show Package Contents`

### Step 2:

GoTo `Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS7.0.sdk/` and copy the file `SDKSettings.plist` to desktop

### Step 3:

Open the file copied `SDKSettings.plist`. Under `<DefaultProperties>` ==> `<dict>`

find `<CODE_SIGNING_REQUIRED>` and change its value from `YES` to `NO`. Save the
file


### Step 4:

Copy this modified SDKSettings.plist file back to  `Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS7.0.sdk/` replacing the orginal file

[YOU MAY SAVE THE ORIGINAL FILE AS BACKUP]

Do the required AUTHENTICATION AS REQUIRED

### Step 5:

Restart `Xcode` and open your runnable xcode project


### Step 6:

In `Project Navigator` select your project and open `Build Settings` section of your porject and Select All sub-heading.


### Step 7:

Under `Code Signing` find `Code Signing Identity` and for both `Debug` and `Release` modes set `Any iOS SDK` to `Don't Code Sign`.


## To make an IPA:

### Step 8:

In `Xcode`, goto `Product` and click `Archive`


### Step 9:

`Step7` will build you project and creat an `Archive`. After the completion of the process, new window `Organize - Archive` will be opened.

In the list of this window you can see your project. Right click project and click `Show in Finder` which will reveal `*.xcarchive` file

### Step 10:

Right click the `*.xcarchive` file and click `Show Package Contents` and goto `Products` => `Applications` where you will see an app file with the name of your project `<projectname>`.app

### Step11: 

Open iTunes change view to Apps and drag the app file `<projectname>.app`  into the iTunes.


### Step12:

Right Click your app, click `Show in Finder`. There you will have you `.ipa file`.


## Important Notes :

1. In Step 8, if the Archive menu is disabled this is most likely because the a simulator option is currently selected as the run target in the Xcode toolbar. Changing this menu either to a connected device, or the generic iOS Device target option should enable the Archive option in Product menu.

2. You will also need to install AppSync in your iPhone via Cydia.


## Xcode 6 越狱开发基础

最近接触到XCode越狱开发的问题，越狱开发首先iphone设备得越狱，然后安装Appsync，安装之后，安装ipa将不再验证程序签名的有效性，不签名的程序也可以直接在设备上运行，只需要保证IPA本身的有效即可。

ipa文件本身即为zip文件，将Xcode编译出的app文件夹放入Payload文件夹中，然后压缩改名即可。

如何编译出一个合适的app文件夹，需要实现在Xcode中选择好SDK，然后将代码签名改成不签名。

在编译目标中选择IOS device，为模拟器编译的app不能在设备上运行，很有可能是因为info.plist中的值不匹配设备。

1. Building setting -> Code signing -> Code Signing Identity 的 Debug 和 Release 全部选择 Don't code sign

2. 顶部导航处，运行按钮后面 active scheme 选择 iOS Device

3. 编译完成以后在 Products 的属性中 选择自己的项目，可以在顶部的导航处找到编译出的app的路径




