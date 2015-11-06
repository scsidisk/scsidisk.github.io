# gitbook 安装在 mac 上

## 安装 ebook-convert

到 http://www.calibre-ebook.com/download 可以看到这种系统下载文件， 下载 mac 版安装包

把 calibre 放入 /Applications 文件夹

创建快捷方式

```
ln -s /Applications/calibre.app/Contents/MacOS/ebook-convert /usr/local/bin/
```

## 安装 gitbook

```
npm install gitbook-cli -g
```

## 使用 gitbook

创建一个文件夹用于存放图书。

创建一个 SUMMARY.md 文件

初始化目录，根据 SUMMARY.md 生成相应的结构和文件

$ gitbook init

可以在浏览器中浏览图书

$ gitbook serve ./repository

也可以生成静态网站:

$ gitbook build ./repository ./outputFolder

## 输出格式

1. 静态网站: This is the default format. It generates a complete interactive static website that can be, for example, hosted on GitHub Pages.
2. PDF: gitbook pdf ./myrepo ./mybook.pdf
3. ePub: gitbook epub ./myrepo ./mybook.epub
4. MOBI: gitbook mobi ./myrepo ./mybook.mobi
5. JSON: This format is used for debugging or extracting metadata from a book. Generate this format using: gitbook build ./myrepo --format=json.

## 安装封面处理插件

下载安装 xquartz

https://xquartz.macosforge.org/landing/

安装 cairo 使用 v 参数查看安装过程的问题，如果 sf 上面的文件下载有问题，可以手动在浏览器中下载

brew install cairo -v

export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:/opt/X11/lib/pkgconfig

npm install gitbook-plugin-autocover

## ibooks 文件路径

open ~/Library/Containers/com.apple.BKAgentService/Data/Documents/iBooks/Books




gitBook 写技术文档
timgertimger 77 2014年09月06日 发布
推荐 0 推荐
收藏 1 收藏，873 浏览
目录结构
gitbook 是一个基于 git 和 github 的静态站点写作工具他们有一个官网上有不少好的书
参见https://www.gitbook.io/

下面介绍下 gitbook 写书的一些记录

界面编辑

下载编辑器 https://github.com/GitbookIO/editor

命令行

下面只记录了主要的命令行使用流程,文章编辑功能就没写啦
http://segmentfault.com/ 贴图片太麻烦

安装

npm -g install gitbook
npm -g install gitbook-plugin
npm install gitbook-plugin-disqus
npm install gitbook-plugin-ga
命令参数

timgerdeMac-mini:PythonToScala_Zh timger$ gitbook

  Usage: gitbook [options] [command]

  Commands:

    build [options] [source_dir] Build a gitbook from a directory
    serve [options] [source_dir] Build then serve a gitbook from a directory
    pdf [options] [source_dir] Build a gitbook as a PDF
    epub [options] [source_dir] Build a gitbook as a ePub book
    mobi [options] [source_dir] Build a gitbook as a Mobi book
    init [source_dir]      Create files and folders based on contents of SUMMARY.md
    publish [source_dir]   Publish content to the associated gitbook.io book
    git:remote [source_dir] [book_id] Adds a git remote to a book repository

  Options:

    -h, --help     output usage information
    -V, --version  output the version number


config

timgerdeMac-mini:PythonToScala_Zh timger$ cat book.json
{
    "plugins": ["ga", "disqus"]
    "pluginsConfig": {
        "ga": {
            "token": "UA-29124639-6"
        },
    "disqus": {
            "shortName": "yishenggudou"
        }
    }
}
build

gitbook build  ./ -o ./build --config=book.json
发布

cp -vrf ../PythonToScala_Zh/build/* ./
git add -f ./*
. ~/.bashrc
. ~/.bash_profile
git_commit_msg "pub"
push_auto_branch
在线地址

http://www.timger.info/PythonToScala/index.html

打包 pdf

timgerdeMac-mini:PythonToScala_Zh timger$ gitbook pdf  ./ -o ./python2scala.pdf --config=book.json
Starting build ...
Successfully built!





Gitbook简明教程
Gitbook是一个命令行工具(node.js库)， 使用Github/Git创建漂亮的图书。 你可以看一些用它编写的图书的例子： 学习Javascript. 你也可以很容易的通过gitbook.io网站发布在线图书。 editor 是一个图形化的编辑工具， 提供Windows, Mac 和Linux的版本. 关注Twitter帐号 @GitBookIO. 这篇文章只是一个起步教程， 完整的文档可以在help.gitbook.io网站找到.

Image
Image

怎么用:

GitBook 可以通过 NPM 安装:

1
$ npm install gitbook -g
你可以将一个repository作为一本书:

1
$ gitbook serve ./repository
或者简单的生成静态网站:

1
$ gitbook build ./repository --output=./outputFolder
命令 build 和 serve 的参数为:

1
2
3
-o, --output <directory>  输出文件件, 默认为 ./_book
-f, --format <name>       产生的书籍的类型, 默认为静态站点, 可用的格式为: site, page, ebook, json
--config <config file>    配置文件, 默认为 book.js 或 book.json
GitBook 会从仓库中的book.json文件加载默认的配置，前提是此文件存在.

下面是此文件的一些配置项:

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
57
58
59
60
61
62
63
64
65
66
67
68
69
70
71
72
73
74
75
76
77
78
79
80
{
    // 输出文件夹
    // 注意: 它会覆盖命令行传入的参数
    // 不建议在此文件中配置
    "output": null,
    // 产生的书籍的类型
    // 注意: 它会覆盖命令行传入的参数
    // 不建议在此文件中配置
    "generator": "site",
    // 图书标题和描述 (默认从README抽取)
    "title": null,
    "description": null,
    // 对于ebook格式, 扩展名the extension to use for generation (default is detected from output extension)
    // "epub", "pdf", "mobi"
    // 注意: 它会覆盖命令行传入的参数
    // 不建议在此文件中配置
    "extension": null,
    // GitHub 信息(defaults are extracted using git)
    "github": null,
    "githubHost": "https://github.com/",
    // 插件列表, can contain "-name" for removing default plugins
    "plugins": [],
    // 插件通用配置
    "pluginsConfig": {
        "fontSettings": {
            "theme": "sepia", "night" or "white",
            "family": "serif" or "sans",
            "size": 1 to 4
        }
    },
    // 模版中的链接 (null: default, false: remove, string: new value)
    "links": {
    	// Custom links at top of sidebar
    	"sidebar": {
    	    "Custom link name": "https://customlink.com"
    	},
        // Sharing links
        "sharing": {
            "google": null,
            "facebook": null,
            "twitter": null,
            "weibo": null,
            "all": null
        }
    },
    // PDF 参数
    "pdf": {
        // Add toc at the end of the file
        "toc": true,
        // Add page numbers to the bottom of every page
        "pageNumbers": false,
        // Font size for the fiel content
        "fontSize": 12,
        // Paper size for the pdf
        // Choices are [u’a0’, u’a1’, u’a2’, u’a3’, u’a4’, u’a5’, u’a6’, u’b0’, u’b1’, u’b2’, u’b3’, u’b4’, u’b5’, u’b6’, u’legal’, u’letter’]
        "paperSize": "a4",
        // Margin (in pts)
        // Note: 72 pts equals 1 inch
        "margin": {
            "right": 62,
            "left": 62,
            "top": 36,
            "bottom": 36
        }
    }
}
输出格式

GitBook可以产生下列类型的图书:

静态站点: 默认格式. 创建一个完全交互式的静态网站，可以发布到GitHub Pages等网站.
eBook: 图书完成后可以使用它创建电子书. 创建命令: gitbook ebook ./myrepo. 你需要安装 ebook-convert. 输出格式可以是 PDF, ePub 或 MOBI.
单页网页: 可以生成一个单页的HTML网页。这个格式可以用来转换PDF或者eBook. 创建命令: gitbook build ./myrepo -f page.
JSON: 此格式用来调试或者抽取图书的元数据. 创建命令: gitbook build ./myrepo -f json.
图书格式

一本图书就是一个Git repository， 至少包含两个文件: README.md 和 SUMMARY.md.

README.md

典型的, 它应该是你的图书的介绍. 它可以自动的被加到最终的summary中.

SUMMARY.md

SUMMARY.md 定义了你的图书的结构. 它应该包含章节的列表,以及它们的链接.

例如:

1
2
3
4
5
6
7
8
9
# Summary
This is the summary of my book.
* [section 1](section1/README.md)
    * [example 1](section1/example1.md)
    * [example 2](section1/example2.md)
* [section 2](section2/README.md)
    * [example 1](section2/example1.md)
不被SUMMARY.md包含的文件不会被gitbook处理.

多语言

GitBook 支持使用多种语言编写图书. 每种语言应该是一个子目录， 遵循正常gitbook格式, LANGS.md文件应该被放到repository的根文件夹， 格式如下:

1
2
3
* [English](en/)
* [French](fr/)
* [Español](es/)
看个例子 学习 Git.

词汇表

允许你列出条目以及它们的定义. 基于这些条目 gitbook会自动创建一个索引，并在页面中加亮这些条目.

GLOSSARY.md 格式很简单 :

1
2
3
4
5
# term
Definition for this term
# Another term
With it's definition, this can contain bold text and all other kinds of inline markup ...
忽略文件和文件夹

GitBook 读取.gitignore, .bookignore 和 .ignore 得到需要忽略的文件/文件夹的列表. (文件的格式和 .gitignore一样).

.gitignore最佳实践是忽略build文件，这些文件来自 node.js (node_modules, ...) 和GitBook的build文件: _book, *.epub, *.mobi and *.pdf.

封面

封面文件为: /cover.jpg.
尺寸为 1800x2360. 插件 autocover可以自动创建一个文件.

封面的小尺寸图形为: /cover_small.jpg.

发布图书

平台 GitBook.io就像"Heroku for books": 你可以在它上面创建图书 (公开的, 付费的, 或者私有的)， 并且使用 git push 就可以更新.

插件

P插件可以扩展图书的功能. 查看插件介绍 GitbookIO/plugin来了解如何创建一个插件.

官方插件:

名称	描述
exercises	Add interactive exercises to your book.
quizzes	Add interactive quizzes to your book.
mathjax	Displays mathematical notation in the book.
mixpanel	Mixpanel tracking for your book
其它插件:

名称	描述
Google Analytics	Google Analytics tracking for your book
Disqus	Disqus comments integration in your book
Autocover	Generate a cover for your book
Transform annoted quotes to notes	Allow extra markdown markup to render blockquotes as nice notes
Send code to console	Evaluate javascript block in the browser inspector's console
Revealable sections	Reveal sections of the page using buttons made from the first title in each section
Markdown within HTML	Process markdown within HTML blocks - allows custom layout options for individual pages
Bootstrap JavaScript plugins	Use the Bootstrap JavaScript plugins in your online GitBook
Piwik Open Analytics	Piwik Open Analytics tracking for your book
Heading Anchors	Add linkable Github-style anchors to headings
JSBin	Embedded jsbin frame into your book
调试

增加环境变量 DEBUG=true 来得到更好的错误信息(包含错误堆栈). 例如:

1
2
$ export DEBUG=true
$ gitbook build ./
评论




gitbook-cli
CLI to generate books using gitbook
The GitBook command line interface.

Install this globally and you'll have access to the gitbook command anywhere on your system.

$ npm install -g gitbook-cli
Note: The job of the gitbook command is to load and run the version of GitBook you have specified in your book (or the latest one), irrespective of its version. The GitBook CLI only support versions >=2.0.0 of GitBook.

How to install it?
$ npm install -g gitbook-cli
How to use it?
Build and Serve
Build a book in the curent directory using:

$ gitbook build
Build a book in another directory:

$ gitbook build ./other_folder
Build and serve the book:

$ gitbook serve ./
List all available commands using:

$ gitbook help
Specify a specific version
By default, GitBook CLI will read the gitbook version to use from the book configuration, but you can force a specific version using --gitbook option:

$ gitbook build ./mybook --gitbook=2.0.1
and list available commands in this version using:

$ gitbook help --gitbook=2.0.1
Manage versions
List installed versions:

$ gitbook versions
List available versions:

$ gitbook versions:available
Install a specific version:

$ gitbook versions:install 2.1.0
Uninstall a specific version

$ gitbook versions:uninstall 2.0.1
Use a local folder as a GitBook version (for developement)

$ gitbook versions:link 2.0.1-alpha ./mygitbook




GitBook: Git + Markdown 快速发布你的书籍
gitbook是一个用于发布个人书籍的平台，类似于国外著名的LeanPub. 其中一个很大的特点是它利用git作为版本管理和发布工具, 加上是在线形式，你可以很方便的进行作为的快速更新.
gitbook提供了一个简单的命令行工具gitbook用来编译和预览的书籍.
NPM
NPM
安装
你可以直接通过npm安装gitbook到全局
1
npm install -g gitbook
gitbook只提供了如下四个命令
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
$ gitbook -h
Usage: gitbook [options] [command]

Commands:

build [options] [source_dir] 编译指定目录，输出Web格式(_book文件夹中)
serve [options] [source_dir] 监听文件变化并编译指定目录，同时会创建一个服务器用于预览Web
pdf [options] [source_dir] 编译指定目录，输出PDF
epub [options] [source_dir] 编译指定目录，输出epub
mobi [options] [source_dir] 编译指定目录，输出mobi
init [source_dir]   通过SUMMARY.md生成作品目录

Options:

-h, --help     output usage information
-V, --version  output the version number
书写
就以我为 regularjs 写的 指南为例，一份gitbook的源文件目录一般是这样的.
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
$ tree -L 2 -U

├── _book
│   ├── index.html
│   ├── en
│   ├── gitbook
│   └── zh
├── node_modules
│   ├── gulp
│   └── gulp-gh-pages
├── en
│   ├── syntax
│   ├── core
│   ├── getting-start
│   ├── advanced
│   ├── SUMMARY.md
│   ├── README.md
│   ├── introduct
│   └── qa
├── zh
│   ├── syntax
│   ├── core
│   ├── getting-start
│   ├── advanced
│   ├── SUMMARY.md
│   ├── README.md
│   └── introduct
├── LANGS.md
├── cover_small.png
├── gulpfile.js
├── package.json
├── book.json
└── README.md
几个关键文件的说明
LANGS.md
当你需要发布多个语言版本时，根目录只需要放置一个LANGS.md, 格式如下
1
2
3
* [English](en)
* [中文](zh)
* ...

每个zh，en文件夹现在就相当于一个独立的书籍.
README.md
REAME相当于书籍的前言部分, 可以忽略
cover_small.png 和 cover.png
书籍的封面图
SUMMARY.md
SUMMARY是最重要的一个部分, 它创建的是整书的索引, 你也可以通过gitbook init读取SUMMARY.md来生成目录结构. 格式如下
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
* [前言](introduct/README.md)
  - [API索引](introduct/index.md)
* [你好 Regular](getting-start/README.md)
  - [安装](getting-start/install.md)
  - [快速起步](getting-start/quirk-example.md)
  - [小结](getting-start/review.md)
* [内建模板引擎](syntax/README.md)
  - [语法说明](syntax/syntax.md)
  - [插值](syntax/inteplation.md)
  - [逻辑控制](syntax/if.md)
  - [循环控制](syntax/list.md)
  - [动态引入](syntax/include.md)
  - [表达式](syntax/expression.md)
  - [小节](syntax/review.md)
* [核心概念](core/README.md)
  - [类式继承和组件定义](core/class.md)
  - [数据监听](core/binding.md)
  - [directive——指令](core/directive.md)
  - [filter——过滤器](core/filter.md)
  - [event——ui事件体系](core/event.md)
  - [regular的模块化策略](core/use.md)
  - [简单事件发射器emitter](core/message.md)
  - [小节](core/review.md)
* [高级特性](advanced/README.md)
  - [内嵌组件](advanced/component.md)
  - [regular的transclude](advanced/content.md)
  - [小节](advanced/review.md)
接下来就是依次完成你每个章节的书写了, 你需要开启gitbook serve .来进行实时的web预览(服务器默认为localhost:400)
markdown的标准格式可以看这里，现在的程序圈的markdown包括gitbook普遍使用的是GitHub Flavored Markdown，除了github中已经说明的那些, 它还支持一些额外的小特性, 比如[x]可以用来设置一个checkbox来实现todolist的功能.
发布
gitbook的命令行工具不提供对发布操作的支持，你可以直接使用git发布，首先你需要添加gitbook的仓库作为你的一个远程库. 比如regularjs的路径为
1
2
3
git remote add gitbook https://push.gitbook.io/leeluolee/regular-guide.git

git push gitbook master
在push成功后，gitbook.io会自动在服务端进行build. 你可以在gitbook.io的个人主页上查看到build信息.
常见问题
gitbook 好卡啊 我可以发布到我的个人网站吗?
当然可以，gitbook build之后的_book 就是一个完整的web目录, 你可以放置到你的个人网站上.
一个更好的做法是直接发布到github的gh-pages上, 由于gitbook每次build都会重新生成整个目录.所以你需要利用gulp-gh-pages或grunt-gh-pages等工具进行发布.
你可以参考我的做法, 这样一键gulp deploy可以完成指定目录_book发布gh-pages.


