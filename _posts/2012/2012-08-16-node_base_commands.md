---
layout: post
title: npm 基本指令
date: 2012-08-16
author: scsidisk
category: Node
tags: Node Node.js npm
---

當你設定好 node.js 的開發環境後, 是時候來把下面這些常用的 npm 指令給摸熟了.

將套件於全域安裝. 全域安裝的套件通常只是為了執行檔而已.

```
$ npm install <package name> -g
# 範例
$ npm install express -g
# 安裝完後現在我們可以用 <code>express</code> 來產生專案
$ express new app
```

將套件安裝在專案裡. 套件在每一個不同的專案裡都要重裝一次不然會 require 不到.$ cd /path/to/the/project

```
$ npm install <package name>
# 範例
$ npm install express
# 現在就可以在專案裡用 `var express = require( 'express' );` 來使用 express 這個套件了.
```

移除全域套件.$ npm uninstall <package name> -g

```
# 範例
$ npm uninstall express -g
```

移除專案裡的套件.

```
$ cd /path/to/the/project
$ npm uninstall <package name>
# 範例
$ npm uninstall express
```

搜尋套件.$ npm search <package name>

```
# 範例
$ npm search express
```

列出全域套件.

```
$ npm ls -g
```

列出全域套件詳細資訊.

```
$ npm ls -gl
```

列出專案裡的套件.

```
$ cd /path/to/the/project
$ npm ls
```

列出專案裡的套件詳細資訊.

```
$ cd /path/to/the/project
$ npm ls -l
```

更新全域套件.$ npm update -g

更新案裡的套件.

```
$ cd /path/to/the/project
$ npm update用 `package.json` 來管理專案裡的套件
```

只要將 package.json 這個檔案放在專案的根目錄裡, 就不需要一個個的手動安裝套件.

原本應該是要

```
$ cd /path/to/the/project
$ npm install mongoose
$ npm install express
$ npm install jade
```

有了 package.json 在專案的根目錄就只要

```
$ cd /path/to/the/project
$ touch package.json
```

package.json

```json
{
    "name": "your app name"
  , "version": "0.0.1"
  , "private": true
  , "dependencies": {
      "express": ">=2.5.0"
    , "jade": ">= 0.16.4"
    , "mongoose": ">=2.3.10"
  }
}
```

然後在 terminal 裡輸入下面的指令就全部安裝完成了.

```
$ npm install -l
```

