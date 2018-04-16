---
layout: post
title: "在 Mac OS X 中创建 RamDisk 用作缓存目录"
date: 2012-04-10
author: Eyon
categories: MacOSX
tags: MacOSX
---


RamDisk 技术，就是通过软件将内存模拟成硬盘来使用。

由于 RamDisk 本身使用的还是内存，可以用于缓存或者临时文件保存方面。

在 OS X 系统中主要存放应用程序缓存的目录，也就是 ~/Library/Caches（大家可以在 Finder 中按快捷键 Command+Shift+G，然后将前面的路径复制进去就知道在哪儿了，~符号在 OS X 中表示当前用户目录的意思），OS X 中大部分应用程序在运行的过程中所产生的缓存文件都会存放在这个文件夹中。所以我们的目的也就是要让 RamDisk 来替代这个目录。

### 创建 RamDisk


#### 要注意的地方

既然是内存，那么关机内容是肯定会丢失的，所以我们仅仅用它来存放可以再生成的缓存，可以再下载的内容也不应该放在里边。比如 OS X 里的 helpd 下载的帮助文件，就不应该放在里边。

你的内存总空间是多少，分配的内存硬盘体积就应该相应地改变大小，毕竟将内存划走了固定的一块，比如我这里用的是1GB，那么我原本的8GB内存就变成了7GB大小。

#### 1. 创建生成脚本

使用 AppleScript 编辑器，粘贴以下代码，运行一下看是否正常（可以在 Finder 左侧看是否有出现 RamDisk 的磁盘）。

```
do shell script "
    // 设置 ram disk 的名称
    RAMDISK=\"RamDisk\"
    // 设置 ram disk 的大小，这里是 1024 MB
    SIZE=1024

    if ! test -e /Volumes/$RAMDISK ; then
        // 分配给 ramdisk 相应大小的空间
        diskutil erasevolume HFS+ $RAMDISK `hdiutil attach -nomount ram://$[SIZE*2048]`
        // 打开元数据索引，如果你使用 Xcode 内部的调试工具这是必须的。因为调试工具使用元数据索引来查询符号连接
        mdutil -i on /Volumes/$RAMDISK
    fi
    mkdir /Volumes/Ramdisk/TempDownloads
    mkdir -p /Volumes/Ramdisk/Library/Developer/Xcode
    mkdir -p /Volumes/Ramdisk/Library/Developer/CoreSimulator/Devices
    mkdir -p /Volumes/Ramdisk/Library/Caches/Google
    mkdir -p /Volumes/Ramdisk/Library/Caches/com.apple.Safari/fsCachedData
    mkdir -p /Volumes/Ramdisk/Library/Caches/com.netease.163music
    mkdir -p /Volumes/Ramdisk/Library/Caches/Firefox

"
```

保存脚本（或者快捷键 command+s），文件格式选择为「应用程序」，保存应用程序文件夹。

![创建 RamDisk](/static/images/2012/04/2770902025.png "创建 RamDisk")

双击执行就可以自动挂载一个叫做“RamDisk”的硬盘了，这个硬盘里的内容会在电脑重启后清空，大小是1GB，如果你把它推出了，那么里边的东西也会丢失，所以务必存放缓存文件。

> 根目录有TempDownloads和一个 Library 文件夹。 Library 文件夹下有 Caches 文件夹和 Developer 文件夹。（Caches 放 Chrome 和 Safari 的浏览器缓存，Developer 放 Xcode 的临时编译空间文件）

#### 2. 设置为启动运行

打开「系统偏好设置——用户与群组」，选择「登录项」选项卡，将你刚刚保存的脚本程序加入到启动列表中即可。

#### 3. 替换 Caches 目录

```
$ rm -rf ~/Library/Caches/Google
$ rm -rf ~/Library/Caches/com.apple.Safari
$ rm -rf ~/Library/Developer/Xcode
$ ln -s /Volumes/Ramdisk/Library/Developer/Xcode/ ~/Library/Developer/Xcode
$ ln -s /Volumes/Ramdisk/Library/Caches/Google ~/Library/Caches/Google
$ ln -s /Volumes/Ramdisk/Library/Caches/com.apple.Safari~/Library/Caches/com.apple.Safari
```

双击 RamDisk 启动。

#### 4. 测试

启动应用程序进行测试，看 RamDisk 目录是否会产生新的文件，如果产生了则证明工作正常。

#### 删除与恢复

如果要删除整个方案，那么只需要先去下面的目录把创建的链接文件删除，再去掉那个脚本的启动项就好了：

    $ sudo rm -rf ~/Library/Developer/Xcode
    $ sudo rm -rf ~/Library/Caches/com.apple.Safari
    $ sudo rm -rf ~/Library/Caches/Google

#### 性能与效果

如果你仅仅做一般的 Xcode 开发，那么 1GB 的空间应该是足够使用的了，这样一来 Xcode 的速度会快很多，不再那么容易就白板了。另外，对于 Chrome 和 Safari 的效果十分的明显。

如果你像我一样经常用 Xcode 来回切换多个开发项目并编译，那么可能会遇到空间塞满的情况，这时候你要注意这个“内存硬盘”不会自己释放空间，所以要手动进去删除一下 Xcode 生成的缓存文件，然后再编译。

往好的方面想，即使重新生成缓存文件，速度也还是比固态要快的，还不会降低硬盘的读写寿命。

#### 建议关闭休眠模式

Mac 默认的休眠模式是3，是一种混合休眠模式，在合盖保存工作到内存时，同时为了防止断电工作丢失，也保存一份到本地磁盘。对于笔记本来说，这个模式实属多余，电量不足还是选择关机吧。模式3意味着将要把内存中的工作保存到磁盘的休眠镜像文件中，这又会造成大量读写。

为了兼容起见，建议直接把模式改为0，就是只启用睡眠模式。（关闭盖子工作只保存到内存），关闭休眠：

    $ sudo pmset-a hibernatemode 0

为了防止该文件再重启后重新生成。（不建议删除，没必要）

    $ sudo touch /private/var/vm/sleepimage
    $ sudo chmod 000 /private/var/vm/sleepimage

其他相关命令：

    $ pmset -g | grep hibernate （查看当前的hibernate模式）
    $ ls -lh /private/var/vm/sleepimage （查看sleepimage文件大小）

今后如果需要打开hibernate模式，再将该值设为默认的就可以了：

    $ sudo pmset -a hibernatemode 3 （设置hibernatemode为默认值3）
    $ sudo rm /private/var/vm/sleepimage

### 重要提示

1. 存放缓存仅仅是 RamDisk 一个比较常用的功能而已，在使用的过程中你完全可以将其当做高速硬盘来使用，比如下载文件的时候直接下载到 RamDisk、将电影存放到 RamDisk 中来看都可以提升性能，而且由于它是对内存进行读写操作，所以可以很大程度上的保护你的硬盘。
2. 建议4GB以上用户才考虑设置 RamDisk，一般来说4GB用户的 RamDisk 大小不超过1GB为好，8GB内存用户可以设置2GB的 RamDisk。
3. 以上方法的脚本代码会设置2GB的 RamDisk，如果你想设置成1GB，请将代码中 ram:// 后面的数字修改成 2097152；如果你想设置成512MB的话则修改成1048576，或者可以直接通过这个网页转换。
4. 在 Mac 中创建 RamDisk 其实一句命令就可以搞定：diskutil erasevolume HFS+ \"RamDisk\" `hdiutil attach -nomount ram://4629672` 。
5. 已经配备 SSD 固态硬盘的用户完全没必要使用 RamDisk，因为 SSD 的速度已经够快了。
6. 切忌将重要文件放到 RamDisk 中。
6. Xcode 编译程序会生成大量的中间文件，一般数百兆甚至更多，放到内存盘中很有必要。
6. 如果要还原三个程序的原本缓存位置，只需再次输入那三条 rm -rf 开头的命令即可。
6. 如果内存盘满了，可以手动清理，或者重启后自动清空。
6. 不建议把整个~/Library/Caches/都链接到内存盘中，因为有一个 com.apple.helped文件夹，只要使用 Xcode 之类的程序，会自动下载帮助文档多达数百兆。每次清理后都会重新下载生成，会严重消耗资源。倒不如让他在硬盘里躺着。
6. 除了这三个程序，其他程序的缓存一般都很小，用不着管他。清除了反而影响程序的启动加载时间。只需要每隔半个月用 cleanMyMac 清理一次即可。（放过那个com.apple.helped，那个文件只要大的不是很夸张多达数 GB，就半年清理一次吧）。
6. 链接到 Ramdisk 的目录，清理软件是不会帮忙清理的，只能手动清理，或者重启自动丢失。

链接： http://9dic.com/ios/2017/05/16/Ramdisk/