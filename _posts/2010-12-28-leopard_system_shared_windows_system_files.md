---
layout: post
title: Leopard系统下共享Windows系统的文件
date: 2010-12-28 13:51
author: scsidisk
category: MacOSX
tags: [Mac, Shell]
Slug: leopard_system_shared_windows_system_files
---

如果你的PC机与Mac机在同一个局域网，可以考虑是否将你的Mac和PC加入到同一个工作组(workgroup)。与Mac
OS 10.4不同，10.4系统的缺省工作组是”Workgroup”，而Mac OS
10.5则是空白。另外，设置工作组的方法也有所不同。在10.4下，工作组的设置是通过实用程序里的”目录访问”(Directory
Access)，双击SMB/CIFS选项后，在SMB/CIFS设置界面上的工作组选项下进行设置或修改的。

在Leopard中，”目录访问”应用已经不存在了，而实用程序下的”目录实用工具”(Directory
Tools)中已经不存在SMB/CIFS服务。工作组的设置任务交给了”系统设置偏好”(System
Preferences)中的网络(Network)设置。在Leopard中设置工作组，可按照以下步骤：

​1. 从苹果菜单或Dock上选择并打开”系统偏好设置”。

​2. 从”系统偏好设置”中找到并点击”网络”(Network)。

​3.
如果网络设置界面左下角的黄色锁标显示为上锁，要双击该图标，并输入管理员用户名和密码解锁。

​4.
从”网络”设置界面，选择已连接的网络服务设备。已连接的网络服务设备名称的左侧有个小绿球型图标。

​5. 点击网络界面右下角的”高级”(Advanced)按键。

​6.
从高级设置界面下，输入或选择工作组名。该工作组名应该于要共享的Windows的工作组名一样。

​7. 点击”好”(OK)，关闭高级设置界面。

​8. 从网络设置界面点击”应用”(Apply)。

​9.
打开一个Finder窗口，同一局域网内的PC机会显示在Finder窗口左侧的”共享的”(Shared)的机器列表下。

​10. 从”共享的”机器列表下，选择要共享的机器。

​11. 从Finder窗口的右栏选择在Finder窗口右上角，点击”连接身份”。

​12. 输入用户名和密码。为方便起见，可以勾选加入钥匙链。

如果要共享的Windows机器与苹果机不在一个局域网或不能使用工作组，则可以使用Finder的”连接服务器”功能。这种方法具体的做法是：

​1. 如果屏幕上方菜单不是Finder菜单，点击桌面将Finder菜单显示出来。

​2.
使用组合键苹果键和k键，或从Finder菜单中的”转到”(Go)下拉菜单里，找到”连接服务器”(Connect
to Server)。

​3. 在连接服务器界面，在服务器地址栏输入： smb://PC的IP地址/共享点名称

​4. 回车或点击”连接”(Connect)键。

​5.
输入PC机的用户名和密码，点击”连接”(Connect)键。一旦验证成功，Finder将用新窗口打开共享点。

注意：WINS是微软的Windows Internet Naming
Service，本身是为的是提供NetBIOS命名，在Windows
2000以后的网络中有没有WINS服务器都没有关系。NetBIOS over
TCP用的是端口137－139，而SMB over
TCP则是端口445。虽然都是用于电脑通过网络来彼此交流，但就本人的理解，NetBIOS和SMB终归是两个不同的概念。也许是为了支持某些落后的网
络系统，苹果在Leopard中的网络设置偏好中增加了WINS的设置，但是，在底层的smb设置文件中，对于Windows下的工作组设置，却采用了
WINS的设置内容，这真让人难以理解。在本文中，实际上没有添加WINS服务器，而只是添加了工作组，却没想到这个修改却令smb的设置中更改了工作组
的数值。

/var/run/smb.config文件为SMB运行设置文件，在[global]下，可以检查workgroup的设置。而Leopard自
身的SMB设置文件为硬盘下的资源库Preferences文件夹SystemConfiguration内的
com.apple.smb.server.plist, 可以用Property List
Editor编辑将其打开，添加或修改Workgroup一项。或者使用终端输入:

sudo defaults write
/Library/Preferences/SystemConfiguration/com.apple.smb.server Workgroup
‘你的工作组名’

<div class="posttagsblock">
</div>

