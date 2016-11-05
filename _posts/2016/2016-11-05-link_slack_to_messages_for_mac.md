---
layout: post
title: 使用 Messages for Mac 连接 Slack
date: 2016-11-05
author: scsidisk
category: MacOSX
tags: Slack
---

如果你想在 Messages for Mac 连接 Slack , 可以参考下面问信息进行设置。

1. 开启 XMPP 支持
-----------------

    进入 <https://my.slack.com/admin/settings#gateways>

    点击 Gateways 后面的 Expand

    选择 Enable XMPP gateway (SSL only) ， 然后 Save

2. 获取 XMPP 信息
-----------------

    进入 <https://my.slack.com/account/gateways>

    查看 Getting Started: XMPP 部分，可以得到 XMPP 信息。

    ![](/static/images/2016/11/link_slack_to_messages_for_mac-00002.png)


3. 设置 Messages for Mac
------------------------

    打开 Messages , 进入 "偏好设置" -> "账户"

    点击左下角 "+" -> "其他信息账户" -> "继续"

    ![](/static/images/2016/11/link_slack_to_messages_for_mac-00001.png)

    设置账户信息：

    ![](/static/images/2016/11/link_slack_to_messages_for_mac-00005.png)


    - 账户类型: Jabber
    - 用户名: mytest@mytest.xmpp.slack.com *此处为 Your XMPP or Jabber ID 参见上面第二步中的信息*
    - 密码: mytest.GixrfEsefPO25PwYq3350z
    - 服务器: mytest.xmpp.slack.com
    - 端口: 空
    - 勾选: 使用 SSL

    完成后点击 "登录", 等一会儿就可以在 "好友" 窗口看到 Slack 已经连接成功, 会显示一些项目列表

    在 "聊天设置" 里面可以设置在聊天是中的昵称.

    ![](/static/images/2016/11/link_slack_to_messages_for_mac-00003.png)

4. 在频道中查看信息
-----------------

    在 Messages 窗口按 ⌘+R 出现聊天室选择响应的 Channel

    ![](/static/images/2016/11/link_slack_to_messages_for_mac-00004.png)

    - 账户: mytest.xmpp.slack.com
    - 聊天室名称: general

    点击左下角 "+" 即可添加聊天室，双击聊天室，进入以后即可等待收取信息。
