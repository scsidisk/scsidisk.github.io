如何编写开源项目的 README 文档
=============================

转自： https://blog.coding.net/blog/how-to-make-readme

[![图片](https://dn-coding-net-production-pp.qbox.me/8797ba54-ffcb-448e-9da9-66fb83df8e3a.png){.bubble-markdown-image}](https://dn-coding-net-production-pp.qbox.me/8797ba54-ffcb-448e-9da9-66fb83df8e3a.png){.bubble-markdown-image-link}

运营一个开源项目就像在运营着一家
Startup，你期待更多人来使用你的项目，并给你的项目加 Star/提交
PR，但好的项目除了其自身真正契合了开发者的需求外，还需要一个好的
README。

> 有好的 README
> 文档的项目不一定是一个好开源项目，但一个好开源项目一定有一个好的
> README。

目前 README 文档编写并没有规范，但一个友好的 README
是有其特征的，我们来看看一个好的 README 的必备要素。

### 国际化问题

首先要注意的是国际化问题，如果你希望自己的项目能获得更多人的使用，提供中英两种
README 文档是非常赞的。你可以在项目头部注明它。如 Coding 的
[WebIDE](https://github.com/Coding/WebIDE#coding-webide) 项目：

[![图片](https://dn-coding-net-production-pp.qbox.me/8d7b5cfb-8cce-4e93-b571-7b3cac2dfb77.png){.bubble-markdown-image}](https://dn-coding-net-production-pp.qbox.me/8d7b5cfb-8cce-4e93-b571-7b3cac2dfb77.png){.bubble-markdown-image-link}

### 项目名及简介

好的项目名及简介是好项目必不可少的。开源项目名不宜过长（除非你有特别的理由这么做），如果你不知道如何给自己的项目起名，可以使用
[随机项目名产生器](http://mrsharpoblunto.github.io/foswig.js/)（适用于
Javascript
项目）；项目简介可以是简单的几句话，但项目简介要说明几个你的开源项目用户想迫切了解的问题，这包括：

-   这个开源项目是做什么的？
-   这个项目是什么语言编写的？
-   项目维护、CI、依赖更新状态（如果有）
-   项目可用版本及其他版本
-   Demo 或官网地址（如果有）

如 Coding 的 [WebIDE](https://github.com/Coding/WebIDE) 项目：

[![图片](https://dn-coding-net-production-pp.qbox.me/572f7336-073b-4b89-b7c1-d04d24e68806.png){.bubble-markdown-image}](https://dn-coding-net-production-pp.qbox.me/572f7336-073b-4b89-b7c1-d04d24e68806.png){.bubble-markdown-image-link}

此外你还可以给项目增加一些图标以提高可读性，推荐使用
[Shields.io](http://shields.io/)

[![图片](https://dn-coding-net-production-pp.qbox.me/2acf3685-41d5-4c66-bc67-3e75723e2e20.png){.bubble-markdown-image}](https://dn-coding-net-production-pp.qbox.me/2acf3685-41d5-4c66-bc67-3e75723e2e20.png){.bubble-markdown-image-link}

### 项目 Logo 和使用截图

你还可以将项目 Logo（如果有的话）放置在 REAME 顶部（这里推荐一个在线制作
Logo 的网站 [Canva](https://www.canva.com) ），项目截图（Gif
动图更佳）也可以帮助你的用户更快速更直观地了解你的开源项目。

[![图片](https://dn-coding-net-production-pp.qbox.me/bb705df8-4f6a-417c-9515-9dfd0e11da7d.gif){.bubble-markdown-image}](https://dn-coding-net-production-pp.qbox.me/bb705df8-4f6a-417c-9515-9dfd0e11da7d.gif){.bubble-markdown-image-link}

### 功能

你可以注明这个项目的功能特点，亮点特色会大大提高访客使用这个项目的概率。

如 Coding 的 [WebIDE](https://github.com/Coding/WebIDE#features) 项目。

[![图片](https://dn-coding-net-production-pp.qbox.me/43d511d5-0fe5-47b2-9ca2-fd4086d2bb31.png){.bubble-markdown-image}](https://dn-coding-net-production-pp.qbox.me/43d511d5-0fe5-47b2-9ca2-fd4086d2bb31.png){.bubble-markdown-image-link}

### 用法

这是 README 中最重要的部分，你需要说明这个项目如何使用，这包括：

-   如何下载这个项目：一般情况下 git clone
    该项目地址即可，当然你也可以提供其他包管理下载安装方式；
-   项目依赖：你需要说明编译运行这个项目前需要哪些依赖；
-   安装：你需要说明如何编译安装/运行这个项目；
-   部署：如果这个项目可以部署的话，请最好注明部署要注意的事项；
-   Debug
    方法：理想状况下，你的用户会顺利编译并运行这个项目，但你要确保用户遇到了问题不会来打扰你（如提交
    Issues），你还需要提供用户可能会遇到的常见问题；

[![图片](https://dn-coding-net-production-pp.qbox.me/869fcff1-fbe5-4ff7-9d61-4f7507169237.png){.bubble-markdown-image}](https://dn-coding-net-production-pp.qbox.me/869fcff1-fbe5-4ff7-9d61-4f7507169237.png){.bubble-markdown-image-link}

### 贡献

对于一个开源项目来说，令其作者最开心的莫过于有人提交 Pull Request
了。加入一个 CONTRIBUTING 文档将大大提高他人贡献你的项目的概率。

你可以说明你的代码规范，项目架构，如何测试和提交 Pull Request
的正确格式，以及其他有利于开发者进行贡献的信息，这将会使你的项目变得更加的规整如一。你可以在项目根目录新建一个
CONTRIBUTING 进行详细的说明并在 README 中添加其文件锚链接。如 Google 的
[Template](https://github.com/GoogleCloudPlatform/Template/blob/master/CONTRIBUTING.md)：

[![图片](https://dn-coding-net-production-pp.qbox.me/8937cd97-43d2-45ce-afd7-f7f6e8f0fb08.png){.bubble-markdown-image}](https://dn-coding-net-production-pp.qbox.me/8937cd97-43d2-45ce-afd7-f7f6e8f0fb08.png){.bubble-markdown-image-link}

### 版权

版权是非常重要的，如果没有声明版权，很多用户特别是企业级用户将受制于法律问题，无法使用你的项目。关于如何选择开源项目许可证，推荐阅读这篇文章：[《如何选择开源许可证？》](http://www.ruanyifeng.com/blog/2011/05/how_to_choose_free_software_licenses.html)

如 Coding 开源的
[帮助文档](https://coding.net/u/coding/p/coding-docs/git) 版权：

[![图片](https://dn-coding-net-production-pp.qbox.me/3a8b21f8-3ed4-43ea-884b-2c42f3092973.png){.bubble-markdown-image}](https://dn-coding-net-production-pp.qbox.me/3a8b21f8-3ed4-43ea-884b-2c42f3092973.png){.bubble-markdown-image-link}

### 鸣谢

你还可以感谢直接或间接为这个项目做出贡献的人、项目。

如 [ttyd](https://github.com/tsl0922/ttyd) 项目：

[![图片](https://dn-coding-net-production-pp.qbox.me/26972118-e8a3-46ce-8f33-909b6fafa0d6.png){.bubble-markdown-image}](https://dn-coding-net-production-pp.qbox.me/26972118-e8a3-46ce-8f33-909b6fafa0d6.png){.bubble-markdown-image-link}

### 其他

我们推荐使用
[Markdown](https://coding.net/help/doc/project/markdown.html) 编写你的
README，请最好注意排版问题以增加文档可读性，推荐阅读 Coding 的
[《文案排版规范》](https://open.coding.net/copywriting.html)。

[![图片](https://dn-coding-net-production-pp.qbox.me/321b6db8-a5e6-4cde-98e6-199d22603a5c.png){.bubble-markdown-image}](https://dn-coding-net-production-pp.qbox.me/321b6db8-a5e6-4cde-98e6-199d22603a5c.png){.bubble-markdown-image-link}

这就是一个好的 README
所需元素了，当然你还可以增加其他任何利于开发者的信息如 Roadmap
等等，这因项目而异。现在，去完善你的开源项目信息或开始做一个开源项目吧！

> 一些建议：选择一个好的代码托管平台/社区可以让你的开源项目获得更多曝光，你可以在
> Coding 的
> [冒泡社区](https://coding.net/pp)（可以理解为程序员的朋友圈）发布你的项目简介，截图和地址，与
> 30 万中国开发者分享你的开源项目；另外我们推荐同时 push 项目到 Coding
> 和 GitHub（可参考
> [该回答](https://segmentfault.com/q/1010000000172591/a-1020000001531401)
> ），得益于 Coding 遍布全国的 CDN，国内用户 clone
> 你的项目时的速度将大大提升。

Happy Coding ; )

（完）