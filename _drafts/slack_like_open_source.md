团队沟通应用 Slack 的五款开源替代品
===================================

2015-11-14 11:19

编译自：
作者： John Light
转载自：开源中国
  
本文地址：<https://linux.cn/article-6575-1.html>

Slack 是非常流行的团队沟通应用，提供群组聊天和直接消息发送功能，支持移动端，Web 和桌面平台。Slack 为用户带来大量的便利，但是也有一些不太好的地方，比如高昂的订阅费用和私有数据泄漏的风险

现在已经有了大量的 Slack 的开源替代品，可以让人们更好的控制数据的安全。那么多的 Slack 替代品如何选择？这里提供了 5 个非常棒的选择：

### Friends

![Friends](https://dn-linuxcn.qbox.me/data/attachment/album/201511/14/112120hbie4iebfaieaa3u.png)

[Friends](http://moose-team.github.io/friends/) 是 Web 平台的 P2P
聊天应用，Slack 的开源替代品。

**技术**

-   纯 JavaScript （NodeJS）
-   GitHub 认证
-   Bonjour (multicast DNS)，本地聊天
-   WebRTC 连接，使用 Hyperlog 进行传播

**优势
**

-   即使中央服务器宕机也可以继续通讯
-   使用 Bonjour 或者 LE Bluetooth 支持离线工作

**劣势
**

-   没有 e2e 加密 DMs
-   通过 GitHub 集中化认证
-   特性限制，支持群组聊天和直接文本通信 + emojis
-   [Empty](https://travis-ci.org/moose-team/friends/builds/78314580#L101) 测试套件

### Let’s Chat

![Let’s
Chat](https://dn-linuxcn.qbox.me/data/attachment/album/201511/14/112211fk8046svvs74v7v6.png)

[Let’s Chat](http://sdelements.github.io/lets-chat) 是由 [Security
Compass](http://securitycompass.com/) 构建的，作为一个 10% time
side-project，是最古老最流行的开源 Slack 替代品，在 GitHub 有着 7300
多的 Stars 和 978 forks。Let's Chat 是一个类似 Slack
的团队聊天软件，基于 Node.js 和 MongoDB 开发，易于发布，适合中小型团队，支持
LDAP/Kerberos 认证，提供 REST 风格 API 和 XMPP 支持。

**技术**

-   后端使用 JavaScript（NodeJS）
-   MongoDB 作为数据存储
-   前端[使用](https://github.com/sdelements/lets-chat/blob/5f689b2d47daae3bfc66e1f6c66d8bb388503324/bower.json#L8)
    Backbone

**优势
**

-   [Hubot](https://hubot.github.com/) 支持
-   在 GitHub 有着庞大的社区
-   大量跟 Slack 相同的特性
-   Security Compass 还在继续开发
-   [Sandstorm](https://sandstorm.io/) [支持](https://blog.sandstorm.io/news/2015-04-13-lets-chat.html)使得自部署更简单，对非技术用户友好

**劣势
**

-   没有 e2e 加密 DMs
-   无原生移动应用
-   无线程转换
-   无测试套件

### Mattermost

![Mattermost](https://dn-linuxcn.qbox.me/data/attachment/album/201511/14/112248zxsx88svs2p6v4ch.png)

[Mattermost](http://mattermost.org/) 是一个 Slack 的开源替代品。Mattermost
采用 Go 语言开发，这是一个开源的团队通讯服务。为团队带来跨 PC
和移动设备的消息、文件分享，提供归档和搜索功能。

**技术**

-   后端使用高性能 Go 语言编写
-   前端使用 React
-   支持 MySQL 和 PostgreSQL

**优势**

-   有一些 Slack 没有的特性
-   原生 [Gitlab
    集成](https://about.gitlab.com/2015/08/18/gitlab-loves-mattermost/)
-   导入 Slack 用户账户，频道文档和主题
-   跟 Slack 使用相同的 webhooks，通过第三方应用发送消息
-   已经为 Docker 容器做准备
-   包含实际测试的测试套件

**劣势**

-   没有 e2e 加密 DMs
-   无原生移动应用
-   无 Sandstorm 应用

### Rocket.Chat

![Rocket.Chat](https://dn-linuxcn.qbox.me/data/attachment/album/201511/14/112318yblxxi6mpafo6zma.png)

[Rocket.Chat](http://rocket.chat/) 是特性最丰富的 Slack
开源替代品之一。主要功能：群组聊天，直接通信，私聊群，桌面通知，媒体嵌入，链接预览，文件上传，语音/视频
聊天，截图等等。Rocket.Chat 原生支持 Windows，Mac OS X ，Linux，iOS 和
Android 平台。Rocket.Chat 通过 hubot 集成了非常流行的服务，比如
GitHub，GitLab，Confluence，JIRA 等等。高级的特性包括：OTR 消息，XMPP
多用户聊天，Kerberos 认证，p2p
文件分享[等等](https://github.com/RocketChat/Rocket.Chat#roadmap)。

**技术**

-   使用 [Meteor](https://www.meteor.com/)，包括
    [Blaze](https://www.meteor.com/blaze) 前端
-   由 JavaScript 和 CoffeeSript 编写
-   MongoDB (because of Meteor)

**优势
**

-   丰富的特性
-   Sandstorm 和 Docker 支持
-   使用 Meteor 创建原生桌面和移动应用
-   支持声音是视频聊天和屏幕分享
-   使用 APIs, hubot 或者 webhooks 来接收第三方服务的通知
-   各种语言本地化

**劣势**

-   没有 e2e 加密 DMs
-   无线程切换
-   几乎是[空的](https://travis-ci.org/RocketChat/Rocket.Chat/builds/88322994)测试套件

### Zulip

![Zulip](https://dn-linuxcn.qbox.me/data/attachment/album/201511/14/112339qgll1dhazplwoabg.jpg)

[Zulip](https://www.zulip.org/) 在被 Dropbox
收购之前是个独立的应用，现在是个开源项目。Zulip
主要特性是群组和直接通信，私有群组交流，线程切换，内联多媒体预览，邮件和桌面通知和[大量的集成](https://zulip.com/integrations)。除了在浏览器运行之外，Zulip
也有原生桌面和移动应用，支持 iOS，Android，Linux Mac 和 Windows。

**技术**

-   服务器使用 Python (Twisted + Django)
-   前端使用 JavaScript + jQuery
-   PostgreSQL, Memcached, Redis, RabbitMQ

**优势**

-   原生桌面和移动应用
-   大量集成 w/ unintrusive 通知
-   线程切换
-   所有 Slack 的特性和 Slack 没有的特性
-   [可扩展](https://travis-ci.org/zulip/zulip)测试套件

**劣势**

-   没有 e2e 加密 DMs
-   无 Sandstorm 应用

 

还有你觉得很不错的 Slack
开源替代品这里没有提到的吗？请在评论中与大家分享吧！

