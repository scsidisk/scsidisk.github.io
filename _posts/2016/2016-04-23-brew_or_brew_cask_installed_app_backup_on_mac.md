---
layout: post
title: "备份 Brew 或 Brew Cask 安装的软件"
date: 2016-04-23
author: scsidisk
categories: OSX
tags: Brew
---

备份安装的软件
-------------

在云存储目录中创建文件夹，保存安装过的程序

    $ mkdir ~/OneDrive/sync/brew/
    $ brew leaves > ~/OneDrive/sync/brew/soft-brew.txt
    $ brew cask list -1 > ~/OneDrive/sync/brew/soft-brew-cask.txt

自动备份
-------

添加下面的命令，每天自动保存安装过的软件列表

    $ crontab -e

    * 1 * * * /usr/local/bin/brew leaves > ~/OneDrive/sync/brew/soft-brew.txt
    * 1 * * * /usr/local/bin/brew cask list -1 > ~/OneDrive/sync/brew/soft-brew-cask.txt

    ## 查看 cron
    $ crontab -l
    ## 备份 cron 列表
    $ crontab -l > backup/crontab.out
    ## 恢复 cron 列表
    $ crontab backup/crontab.out

恢复备份
-------

恢复备份的安装列表，执行下面的命令

    $ brew install `cat ~/OneDrive/sync/brew/soft-brew.txt`
    $ brew cask install `cat ~/OneDrive/sync/brew/soft-brew-cask.txt`