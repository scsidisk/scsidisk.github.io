---
layout: post
title: 用 npm script webpack 自动化任务
date: 2016-04-10
author: scsidisk
category: Javascript
tags: npm, webpack, javascript
---

使用 npm script + webpack 完成前端自动化任务，配置起来也很简单。

npm
----

1、npm安装；
2、生成配置文件

    npm init

会在当前目录生成 package.json 文件，其中:

- scripts：指定了运行脚本命令的npm命令行缩写，比如start指定了运行npm run start时，所要执行的命令。
- dependencies ：依赖的模块。npm install xxx --save  （安装并写入 package.json）
- devDependencies ：开发所需要的模块。 npm install xxx --save-dev （安装并写入package.json）

npm script
----------

npm 会在项目的 package.json 文件中寻找 scripts 区域，其中包括npm test和npm start等命令。

其实npm test和npm start是npm run test和npm run start的简写。事实上，你可以使用npm run来运行scripts里的任何条目。

使用npm run的方便之处在于，npm会自动把node_modules/.bin加入$PATH，这样你可以直接运行依赖程序和开发依赖程序，不用全局安装了。只要npm上的包提供命令行接口，你就可以直接使用它们，方便吧？当然，你总是可以自己写一个简单的小程序。

Webpack
-------

webpack 可以作为全局的npm模块安装，也可以在当前项目中安装。

    npm install -g webpack
    //npm install --save-dev webpack


模块加载器兼打包工具，它能把各种资源，例如JS（含JSX）、coffee、样式（含less/sass）、图片等都作为模块来使用和处理。采用commonJS形式编写，通过 require 引入各个组件

配置文件为 webpack.config.js

    module.exports = {
      entry: {
        page1: "./page1",
        //支持数组形式，将加载数组中的所有模块，但以最后一个模块作为输出
        page2: ["./entry1", "./entry2"]
      },
      output: {
        path: "dist/js/page",
        publicPath: "/dist/js/page",
        filename: "[name].bundle.js"
      },
      module: {
        loaders: [
          //.css 文件使用 style-loader 和 css-loader 来处理
          { test: /\.css$/, loader: 'style-loader!css-loader' },
          //.js 文件使用 jsx-loader 来编译处理
          { test: /\.js$/, loader: 'jsx-loader?harmony' },
          //.scss 文件使用 style-loader、css-loader 和 sass-loader 来编译处理
          { test: /\.scss$/, loader: 'style!css!sass?sourceMap'},
          //图片文件使用 url-loader 来处理，小于8kb的直接转为base64
          { test: /\.(png|jpg)$/, loader: 'url-loader?limit=8192'}
        ]
      },
      resolve: {
        //查找module的话从这里开始查找
        root: '/pomy/github/flux-example/src', //绝对路径
        //自动扩展文件后缀名，意味着我们require模块可以省略不写后缀名
        extensions: ['', '.js', '.json', '.scss'],
        //模块别名定义，方便后续直接引用别名，无须多写长长的地址
        alias: {
          AppStore : 'js/stores/AppStores.js',//后续直接 require('AppStore') 即可
          ActionType : 'js/actions/ActionType.js',
          AppAction : 'js/actions/AppAction.js'
        }
      }
    }

entry 是页面入口文件配置，output 是对应输出项配置（即入口文件最终要生成什么名字的文件、存放到哪里）

该段代码最终会生成一个 page1.bundle.js 和 page2.bundle.js，并存放到 ./dist/js/page 文件夹下

output参数是个对象，定义了输出文件的位置及名字：

- path: 打包文件存放的绝对路径
- publicPath: 网站运行时的访问路径
- filename:打包后的文件名

plugins 是插件项, module.loaders 是最关键的一块配置。它告知 webpack 每一种文件都需要使用什么加载器来处理

test项表示匹配的资源类型，loader或loaders项表示用来加载这种类型的资源的loader，loader的使用可以参考 using loaders，更多的loader可以参考 list of loaders。

！用来定义loader的串联关系，”-loader”是可以省略不写的，多个loader之间用“!”连接起来，但所有的加载器都需要通过npm来加载。

resolve: webpack在构建包的时候会按目录的进行文件的查找，resolve属性中的extensions数组中用于配置程序可以自行补全哪些文件后缀.

然后我们想要加载一个js文件时，只要require(‘common’)就可以加载common.js文件了。

注意一下, extensions 第一个是空字符串! 对应不需要后缀的情况.

构建javascript
--------------

为了便于组织代码和利用npm上的包，写代码的时候往往使用module.exports和require()。browserify可以将这些一起打包成单一的脚本。使用browserify很简单，只需在package.json中加入一个['build-js']条目，类似这样：

    "build-js": "browserify browser/main.js > static/bundle.js"

如果是用于生产环境，还需要压缩一下。我们只需要将uglify-js加入devDependency，然后直接通过管道传递一下即可：

    "build-js": "browserify browser/main.js | uglifyjs -mc > static/bundle.js"

监视 javascript
---------------

为了能在修改文件之后自动重新生成javascript文件，只需将上面的browserify命令换成watchify并加上一些参数。

    "watch-js": "watchify browser/main.js -o static/bundle.js -dv"

这里加了-d和-v两个参数，这样就可以看到详细的调试信息。

构建CSS
--------

用cat就可以搞定：

    "build-css": "cat static/pages/*.css tabs/*/*.css > static/bundle.css"

监视CSS
-------

和上面用 watchify 监视 javascript 类似，我们用catw监视CSS文件的改动：

    "watch-css": "catw static/pages/*.css tabs/*/*.css -o static/bundle.css -v"

序列化子任务
----------

很简单，npm run每个子任务，然后用&&连接起来就成。

    "build": "npm run build-js && npm run build-css"

并行子任务
---------

类似地，我们用&并行子任务：

    "watch": "npm run watch-js & npm run watch-css"

完整的package.json例子
---------------------

将上面提到的内容组合起来，package.json大致就是这个样子：

    {
      "name": "my-silly-app",
      "version": "1.2.3",
      "private": true,
      "dependencies": {
        "browserify": "~2.35.2",
        "uglifyjs": "~2.3.6"
      },
      "devDependencies": {
        "watchify": "~0.1.0",
        "catw": "~0.0.1",
        "tap": "~0.4.4"
      },
      "scripts": {
        "build-js": "browserify browser/main.js | uglifyjs -mc > static/bundle.js",
        "build-css": "cat static/pages/*.css tabs/*/*.css",
        "build": "npm run build-js && npm run build-css",
        "watch-js": "watchify browser/main.js -o static/bundle.js -dv",
        "watch-css": "catw static/pages/*.css tabs/*/*.css -o static/bundle.css -v",
        "watch": "npm run watch-js & npm run watch-css",
        "start": "node server.js",
        "test": "tap test/*.js"
      }
    }

生产环境下，只需运行npm run build。如果是本地开发，就用npm run watch。

你也可以坐下扩展。比方说，如果你希望在运行start前先运行build，那么你只需写上这么一行：

    "start": "npm run build && node server.js"

也许你想同时启动watcher？

    "start-dev": "npm run watch & npm start"

当事情变得非常复杂的时候
---------------------

如果你发现在单个scripts条目中塞了一大堆命令，那你可以考虑重构一下，把一些命令放到别的地方，比如/bin。

你可以用任何语言编写这个脚本，比如bash、node或perl。只需要在脚本上加上合适的#!行。还有，别忘了chmod +x。

    #!/bin/bash
    (cd site/main; browserify browser/main.js | uglifyjs -mc > static/bundle.js)
    (cd site/xyz; browserify browser.js > static/bundle.js)
    "build-js": "bin/build.sh"

Windows
--------

你可能会吃惊的是，相当多的类bash语法可以在Windows上工作。不过我们至少还需要让;和&可以正常工作。

James Halliday分享过一些在Windows兼容的经验，这些经验也适用于本文的主题，可以参考。此外要推荐下win-bash，这是一个很方便的Windows平台上的bash实现。

总结
----

James Halliday希望这个使用npm run的方式能吸引一部人对现有的前端自动化任务工具不满意的人。James Halliday比较偏好unix体系下的那些学习曲线陡峭的工具，比如git，或者类似 npm 这种在 bash 的基础上提供极简界面的工具。也就是说，不需要很多仪式化操作和配合的工具。非常简单的工具，已经足够胜任通常的任务。

如果你对npm run风格不感冒。你也许可以考虑下Makefiles，一个稳定而简单，不过多少有点怪异的替代品。

