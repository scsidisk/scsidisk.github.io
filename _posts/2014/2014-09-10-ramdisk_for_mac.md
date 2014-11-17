---
layout: post
title: 在 Mac OS X 中创建 RamDisk 用作缓存目录
date: 2014-09-10
author: Eyon
category: MacOSX
tags: MacOSX
---

![](/images/2014/09/ramdisk-01.png)

应该有很多读者都知道 RamDisk 这项技术，简单的说就是通过软件将内存模拟成硬盘来使用的技术，因为内存的读写速度都要比硬盘快很多很多倍（至少是10倍以上），所以将内存当做硬盘用能够非常明显的提升系统新能。

由于 RamDisk 本身使用的还是内存，所以即便模拟成硬盘之后，也难逃重启系统之后一切数据都将丢失的命运。其实也正是这一特性让它最大的用武之地在缓存或者临时文件保存方面，因为这些数据往往并不需要长期保存。

OS X 系统主要存放应用程序缓存的目录，也就是 `~/Library/Caches`（大家可以在 Finder 中按快捷键 Command+Shift+G，然后将前面的路径复制进去就知道在哪儿了，`~` 符号在 OS X 中表示当前用户目录的意思）这个目录，OS X 中大部分应用程序在运行的过程中所产生的缓存文件都会存放在这个文件夹中。所以我们的目的也就是要让 RamDisk 来替代这个目录。

### 方法一（操作简单，但需花费18元人民币）：

在 Mac App Store 中购买 iRamDisk 这个应用，它可以非常方便的创建指定大小的 RamDisk，并且能够直接设置成在开机/登录时自动创建 RamDisk。大家可以直接根据下图设置，然后点击 Create！

![](/images/2014/09/ramdisk-02.png)

接下来删除 ~/Library/Caches 这个目录，你可以直接在 Finder 中完成，也可以在终端(应用程序/实用工具)中执行命令: `sudo rm -rf ~/Library/Caches`，执行命令过程中会让你输入用户密码。大家在执行命令之前请记得尽可能的退出所有在运行中的程序（只保留 Finder 和”终端”最好）。

完成删除之后，再在「终端」中执行命令：`ln -s /Volumes/RamDisk/ ~/Library/Caches`，这一步的目的是在原有的缓存目录处建立 RamDisk 的”替身”，也就是再往 `~/Library/Caches` 目录存放文件就会直接存放到 RamDisk 中。

启动应用程序进行测试，Safari、Chrome、iTunes 什么的都行，看启动应用程序之后 RamDisk 目录是否会产生新的文件（夹），如果产生了则证明工作正常。

![](/images/2014/09/ramdisk-03.png)

### 方法二（操作稍微有一丁点复杂，完全免费）：

启动「AppleScript 编辑器」，这个工具也是在「应用程序/实用工具」目录下，建议大家直接从 OS X 右上角的搜索框中搜索(`Spotlight`)启动；
粘贴以下代码到 AppleScript 脚本区，并点击窗口中的运行，看是否能够正常执行（可以在 Finder 左侧看是否有出现 RamDisk 的磁盘），如果能够正常执行的话开始下一步。

```
do shell script "
if ! test -e /Volumes/\"Ramdisk\" ; then
diskutil erasevolume HFS+ \"RamDisk\" `hdiutil attach -nomount ram://4629672`
fi
"
```

点击菜单栏的文件——存储为（或者快捷键 `command+shift+s`），在存储为对话框中将文件格式选择为「应用程序」，如果确定脚本代码没有问题的话可以勾上”仅运行”复选框，然后将其保存到你想要的位置。

![](/images/2014/09/ramdisk-04.png)

其实上面的过程就是生成一个可以创建 RamDisk 的脚本程序，接下来我们还需要让脚本程序在启动/登陆时自动运行。打开「系统偏好设置——用户与群组」，选择「登录项」选项卡，将你刚刚保存的脚本程序加入到启动列表中即可。

剩下的步骤与「方法一」中的2、3、4步完全一样。

### 重要提示

存放缓存仅仅是 RamDisk 一个比较常用的功能而已，在使用的过程中你完全可以将其当做高速硬盘来使用，比如下载文件的时候直接下载到 RamDisk、将电影存放到 RamDisk 中来看都可以提升性能，而且由于它是对内存进行读写操作，所以可以很大程度上的保护你的硬盘。

建议4GB以上用户才考虑设置 RamDisk，一般来说4GB用户的 RamDisk 大小不超过1GB为好，8GB内存用户可以设置2GB的 RamDisk。

以上方法2中的脚本代码会设置2GB的 RamDisk，如果你想设置成1GB，请将代码中 `ram://` 后面的数字修改成 2097152；如果你想设置成512MB的话则修改成1048576，或者可以直接通过这个网页转换。

在 Mac 中创建 RamDisk 其实一句命令就可以搞定：``diskutil erasevolume HFS+ \"RamDisk\" `hdiutil attach -nomount ram://4629672` ``方法二中的前面几个步骤都是为了能让脚本在进入系统时自动启动。

已经配备 SSD 固态硬盘的用户完全没必要使用 RamDisk，因为 SSD 的速度已经够快了。

切忌将重要文件放到 RamDisk 中。

转自：http://www.guomii.com/posts/24462
