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

#### 1. 创建生成脚本

使用 AppleScript 编辑器，粘贴以下代码，运行一下看是否正常（可以在 Finder 左侧看是否有出现 RamDisk 的磁盘）。

```
do shell script "
if ! test -e /Volumes/\"Ramdisk\" ; then
diskutil erasevolume HFS+ \"RamDisk\" `hdiutil attach -nomount ram://4629672`
fi
"
```

保存脚本（或者快捷键 command+s），文件格式选择为「应用程序」，保存应用程序文件夹。

![创建 RamDisk](/static/images/2012/04/2770902025.png "创建 RamDisk")

#### 2. 设置为启动运行

打开「系统偏好设置——用户与群组」，选择「登录项」选项卡，将你刚刚保存的脚本程序加入到启动列表中即可。

#### 3. 替换 Caches 目录

```
$ sudo rm -rf ~/Library/Caches
$ ln -s /Volumes/RamDisk/ ~/Library/Caches
```

双击 RamDisk 启动。

#### 4. 测试

启动应用程序进行测试，看 RamDisk 目录是否会产生新的文件，如果产生了则证明工作正常。

### 重要提示

1. 存放缓存仅仅是 RamDisk 一个比较常用的功能而已，在使用的过程中你完全可以将其当做高速硬盘来使用，比如下载文件的时候直接下载到 RamDisk、将电影存放到 RamDisk 中来看都可以提升性能，而且由于它是对内存进行读写操作，所以可以很大程度上的保护你的硬盘。
2. 建议4GB以上用户才考虑设置 RamDisk，一般来说4GB用户的 RamDisk 大小不超过1GB为好，8GB内存用户可以设置2GB的 RamDisk。
3. 以上方法的脚本代码会设置2GB的 RamDisk，如果你想设置成1GB，请将代码中 ram:// 后面的数字修改成 2097152；如果你想设置成512MB的话则修改成1048576，或者可以直接通过这个网页转换。
4. 在 Mac 中创建 RamDisk 其实一句命令就可以搞定：diskutil erasevolume HFS+ \"RamDisk\" `hdiutil attach -nomount ram://4629672` 。
5. 已经配备 SSD 固态硬盘的用户完全没必要使用 RamDisk，因为 SSD 的速度已经够快了。
6. 切忌将重要文件放到 RamDisk 中。

转自：http://www.guomii.com/posts/24462