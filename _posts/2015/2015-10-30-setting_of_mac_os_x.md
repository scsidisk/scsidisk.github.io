---
layout: post
title: "Mac 的一些有用的设置"
date: 2015-10-30
author: scsidisk
categories: MacOSX
tags: MacOSX
---

Mac 的一些有用的设置

### 删除 DS_Store

```
# 删除
find ./ -name ".DS_Store" -depth -exec rm {} \;
# 设置不在产生
defaults write com.apple.desktopservices DSDontWriteNetworkStores true
# 恢复产生
defaults write com.apple.desktopservices DSDontWriteNetworkStores

```