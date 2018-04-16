使用 Package Syncing 同步 Sublime Text 3 的偏好设置和扩展包 
===========================================================

August 29, 2016

这篇文章就要介绍一种更为简便的方法，来让你的
Sublime Text 的个人配置在不同设备上都能够保持同步。

同步偏好设置和扩展包的原理 
--------------------------

我们并不需要对 Sublime Text
的所有文件都进行同步，只需要同步其中的
`~/Library/Application\ Support/Sublime\ Text\ 3/Packages/User/`
路径，这个路径也可以在启动 Sublime Text 3 后通过菜单选取“Sublime
Text”&gt;“Preferences”&gt;“Settings - User”查看。

> `User`
> 路径中所存储的 `Package Control.sublime-settings`
> 文件已经记录了我们所安装的所有扩展包的名称。

安装 Package Syncing 扩展包 
---------------------------

[Package
Syncing](https://packagecontrol.io/packages/Package%20Syncing "Package Syncing - Packages - Package Control")
是一个由
[csch0](https://packagecontrol.io/browse/authors/csch0 "csch0 - Authors - Package Control")
为 Sublime Text 所开发的扩展包，在安装后，我们无需在 Terminal
中使用命令行的方式进行配置，只需要修改该扩展包的配置文件即可。

![由 csch0 开发的 Sublime Text 扩展包 Pacakge
Syncing](/static/images/2016/08/introducing-package-syncing.png "由 csch0 开发的 Sublime Text 扩展包 Pacakge Syncing")

现在，我们需要在 Sublime Text 3 中调出 [Package
Control](https://packagecontrol.io "Package Control - the Sublime Text package manager")，以
macOS 为例，按下组合键
`Shift-Command-P`^

> 不知道什么是 Package Control？如果没有安装 Package
> Control，我想基本上就没有了使用 Sublime Text
> 的意义，应该也几乎没有人会这样做，因此在这里就不再赘述如何安装 Package
> Control（想要了解详细方法的读者请自行阅读脚注 \[1\]）。坚持不安装
> Package Control 的话，请阅读 [Package Syncing
> 文档](https://packagecontrol.io/packages/Package%20Syncing "Package Syncing - Packages - Package Control")
> 中的 Not using Package Control 一节。

![不使用 Package Control 安装 Package
Syncing](/static/images/2017/11/not-using-package-control.png "不使用 Package Control 安装 Package Syncing")

在 Package Control 中查找到“Package Control: Install
Package”一项并选择，然后输入“Package Syncing”，找到这个由 csch0
所开发的扩展包，选择并等待安装完成。

![使用 Package Control 安装 Package
Syncing](/static/images/2017/11/install-package-syncing-using-package-control.png "使用 Package Control 安装 Package Syncing")

配置 Package Syncing 扩展包 
---------------------------

安装完成后，通过菜单选取“Sublime Text”&gt;“Preferences”&gt;“Package
Settings”&gt;“Package Syncing”&gt;“Settings -
User”，然后输入以下内容并保存。

``` 
{
    "sync": true,
    "sync_folder": "/Users//Library/Mobile Documents/com~apple~CloudDocs/Sublime Text"
}
```

请注意需要将 `sync_folder` 键所对应值中的
``
替换为你的个人文件夹的名称。例如，我的个人文件夹的名称为
`Cherysun`，则我需要将该值写为
`/Users/Cherysun/Library/Mobile Documents/com~apple~CloudDocs/Sublime Text`。

该路径是我们使用 iCloud Drive 进行同步的路径，即将
`~/Library/Application\ Support/Sublime\ Text\ 3/Packages/User/`
文件夹保存在 iCloud Drive 中。如果你希望将其同步到使用 Google Drive 或者
OneDrive、Dropbox 等其他服务，那么可以根据个人需要对该路径进行修改。

将偏好设置和扩展包从 iCloud Drive 同步到其他设备 
------------------------------------------------

当我们完成了上述配置后，本地的 Sublime Text
偏好设置和扩展包就会被自动同步到 iCloud
Drive。如果我们希望在另外的设备上将该内容同步下来，那么只需要在安装好
Sublime Text 和 Package Control 后，接着安装 Package Syncing
扩展包然后重写一遍该配置，Package Syncing
就会从同步的路径中读取已同步的偏好设置，然后将缺少的扩展包自动安装和配置好。



