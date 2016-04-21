---
layout: post
title: Mac OS X 禁用 Dashboard
date: 2010-12-28 13:51
author: scsidisk
category: MacOSX
tags: [Mac]
Slug: mac_os_x_dashboard_disabled
---

打开 Terminal 敲入以下命令：

defaults write com.apple.dashboard mcx-disabled -boolean YES

然后你可以重启macbook或者敲入以下命令：

killall Dock

当你又需要dashboard的时候，以下命令可以逆转：

defaults write com.apple.dashboard mcx-disabled -boolean NO

再重启或敲入 :

killall Dock

<div class="posttagsblock">
</div>

