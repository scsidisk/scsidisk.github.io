---
layout: post
title: "如何在 Mac OSX Lion 上設定 node.js 的開發環境"
date: 2012-08-15
author: scsidisk
categories: Node
tags: Node
---

在 Mac 上建立一個 node.js 的開發環境。

### 用 Homebrew 來安裝及更新 node.js

要在 Mac 上建立一個 node.js 的開發環境有很多方法. 你可以直接下載原始碼自己編譯, 或者是用套件管理系統來幫你解決這些瑣碎的問題. 因為 node.js 還是一個很年輕的專案, 常常會有版本的更新. 手動安裝及更新實在是非常的累人. 若是使用 Homebrew 來幫你處理這些問題可以讓你把時間花在寫程式而不是設定環境上面. 如果你是使用 Ubuntu 的話可以參考這一篇文章.

### 安裝 Xcode

什麼? 我只不過是想寫 server side javascript 而已為什麼要安裝 4.3 GB 的Xcode 4? 嗯… 因為你需要 gcc 來編譯 node.js 和其他的套件. 所以還是乖乖裝吧…

### 安裝 Homebrew

Homebrew 是我在 Mac 上最喜歡的套件管理系統. 他就像是 Ubuntu 上的 apt-get. 我們會需要他來幫我們安裝 node.js 以及 mongoDB. 如果你還沒聽過他的話現在趕快來試試看吧!

```
$ /usr/bin/ruby -e "$(/usr/bin/curl -fsSL https://raw.github.com/mxcl/homebrew/master/Library/Contributions/install_homebrew.rb)"
```

我發現用 nvm( node version management ) 來安裝 node 簡單多了, 他是一個像是 ruby rvm 的東西. 可以讓你切換 node 的版本以利在開發時切換版本. 還有 npm 在 node 0.6.3 之後已經直接包在 node 裡面不需另外安裝了. 所以基本上後面 安裝 npm 可以跳過不看.

### 安裝 node.js

用 Homebrew 來安裝 node.js 非常簡單. 只要下面兩行指令就搞定了.

我比较喜欢使用 hrew 管理软件，所以其他安装方法忽略

```
$ brew update
$ brew install node
```

### 安裝 npm

npm 是 node.js 最受歡迎的套件管理系統. 就像是 ruby 的 gem 以及 php 裡的 pear.

現在上面已經有幾千個現成套件了. 包括 ORM, router, 以及第三方 api 的 wrapper 等等. 所以當你在寫新功能之前先上
npm 找找是不是已經有現成的模組可用吧.

```
$ curl http://npmjs.org/install.sh | sudo sh
```

### 安裝 mongodb

mongoDB 是我首選的 NoSQL 資料庫. 雖然他不是裡面最快的但卻是最好上手以及使用的一個.

尤其是對習慣關聯式資料庫的人來說更是如此. 但是千萬不要用設計關聯式資料庫資料結構的思維來設計你的 NoSQL
資料結構, 不然你的 node.js 程式跑起來還是快不到哪去的. 記得在安裝之後好好看一下他寫的非常詳盡的文件.

```
$ brew install mongodb
# create db directory
$ mkdir /usr/local/db
```

### 更新 node.js

一樣用上面的指令就可以安裝新版本的 node 並且可以在版本中切換..

### 更新 mongoDB

用 Homebrew 來更新 mongoDB 非常的容易. 下面兩行指令就幫你搞定了.

```
$ brew update
$ brew upgrade
```

現在你已經設定好 node.js 的開發環境了. 跟著可以來看看如何使用 npm 以及一些基本的 javascript 和 node.js.