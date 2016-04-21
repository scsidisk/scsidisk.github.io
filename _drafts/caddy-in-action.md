Caddy 部署实践
=============

https://wuwen.org/2014/11/13/caddy-in-action


TL’DR [Caddy](https://caddyserver.com/) 就是用 Go 语言实现的一款 Web
服务器

我最早接触到 Caddy 是偶然发现国内某个博客的简单介绍，然后上到 GitHub
一看发现竟然认识作者（一起在 GopherCon 吃过薯片算不算 😂），接着出于对 Go
写的程序的天然好感，于是就一直琢磨着什么时候可以赶个时髦把这货用上。

机缘巧合下，[Gogs](https://gogs.io) 项目前段时间得到 DO
的开源项目服务器赞助（顺便吐槽国内各大“云”主机各种无视请求邮件），于是就把我的服务器迁移到
DO 新开的加拿大机房，并且使用 Caddy 来替代 NGINX 作为我的 Web
服务器。踩到一些小坑，但整个过程还是挺满意的。

废话不多说，下面直接切入正题。

下载安装步骤简单粗暴，你可以直接从
[官网](https://caddyserver.com/download) 直接下载编译好的二进制。

但是我们这里会介绍到如何安装插件，所以还是从源码安装比较合适（这里使用
`-d` 表示只下载不编译）：

    $ go get -d github.com/mholt/caddy

接着，安装 Caddy 的插件管理器
[caddyext](https://github.com/pedronasser/caddyext)：

    $ go get github.com/pedronasser/caddyext

我们可以先查看一下内置的插件都有哪些：

    $ caddyext stack
    Available Caddy directives/extensions:
       (✓) ENABLED | (-) DISABLED

       0. (✓) root (core)
       1. (✓) tls (core)
       2. (✓) bind (core)
       3. (✓) startup (core)
       4. (✓) shutdown (core)
       5. (✓) log (core)
       6. (✓) gzip (core)
       7. (✓) errors (core)
       8. (✓) header (core)
       9. (✓) rewrite (core)
       10. (✓) redir (core)
       11. (✓) ext (core)
       12. (✓) mime (core)
       13. (✓) basicauth (core)
       14. (✓) internal (core)
       15. (✓) proxy (core)
       16. (✓) fastcgi (core)
       17. (✓) websocket (core)
       18. (✓) markdown (core)
       19. (✓) templates (core)
       20. (✓) browse (core)

接下来安装 [git](https://caddyserver.com/docs/git) 插件（安装
[其它插件](https://caddyserver.com/docs) 的步骤是一样的）。

首先，你需要下载这个插件（🙄
坑了我半天，文档也妈蛋地没说，错误提示更是神乎其神）：

    $ go get github.com/abiosoft/caddy-git

然后，通过 caddyext 安装插件的语法是
`caddyext install <自定义插件名称> <插件导入路径>`：

    $ caddyext install git github.com/abiosoft/caddy-git

检查一下是否安装成功：

    $ caddyext stack
    Available Caddy directives/extensions:
       (✓) ENABLED | (-) DISABLED

       ...
       18. (✓) markdown (core)
       19. (✓) templates (core)
       20. (✓) browse (core)
       21. (✓) git

不错，我们看到 git 是排位第 21 的插件。

这个数字表示插件的执行顺序，你可以使用 `caddyext move`
来改变默认顺序。不过内置插件直接有相互依赖关系，最好不要乱改内置插件的前后顺序。

除此之外，你还可以禁用某个插件等等，这里不做赘述。

此刻，终于可以编译我们的 Caddy 服务器啦！

    $ go install github.com/mholt/caddy

如果你已经将 `$GOPATH/bin` 加入到环境变量 `$PATH` 中，那么可以直接执行
`caddy` 启动服务器：

    $ caddy
    [INFO] Processing certificate renewals...
    0.0.0.0:2015

Caddy 的默认端口是 2015（因为作者想要强调这才是 2015 年应该使用的 Web
服务器 💀）。

如果这个时候你访问 <http://localhost:2015> ，你会看到无比坑爹的
`404 Not Found`。连个欢迎页面都没有，简直不能忍。据作者说这是故意的，如果像
NGINX 那样放个欢迎页面就好比脱裤子放屁。。。😳

没事，我忍了，但是个人喜欢 NGINX 那样的提示，可以用来证明 Caddy
在我服务器正常运行。然后，我就利用它自带的功能 hack 一个简陋的欢迎页面。

将下面的内容保存到当前目录（也就是你正在执行 Caddy 的目录）下，命名为
`index.html`：

    <!DOCTYPE html>
    <html>
        <head>
            <title>Welcome to Caddy</title>
        </head>
        <body>
            <h1>Welcome to Caddy</h1>
        </body>
    </html>

然后重启 Caddy，再次访问 <http://localhost:2015> 。哈哈，请给我的机智点
32 个 👍！

那么，如果是实际部署在服务器，你需要创建一个 `Caddyfile`
并输入以下内容让它监听在 `80` 端口（真的是简洁到爆有木有！）：

    :80

接下来的我就主要讲一下有关 [Pugo.Static](http://pugo.io)
的部署部分（也就是你正在浏览的这个玩意）。

我博客的域名是 `wuwen.org`，目前还没上 HTTPS，所以完整的 URL 就是
`http://wuwen.org`。

我启动 Pugo.Static 的端口是默认的 `9899`，所以，我需要像 NGINX
那样，反向代理所有到 `http://wuwen.org` 的请求转发给
`http://localhost:9899`。

献上我珍藏已久的配置（这里用到了
[proxy](https://caddyserver.com/docs/proxy) 插件）：

    http://wuwen.org {
        proxy / localhost:9899
    }

我要是不说，你可能都已经忘记我们刚才还装了 git 插件 😂
那么我们要怎么用呢？

    http://wuwen.org {
        proxy / localhost:9899
        git https://github.com/Unknwon/wuwen.pugo.git /path/to/blog {
            interval 60
        }
    }

意思很简单，每隔 60 秒从地址 `https://github.com/Unknwon/wuwen.pugo.git`
拉取内容到指定目录 `/path/to/blog`。

好了，如此一来，我的博客不仅可以每分钟自动同步，还有了版本管理，并且自带云备份属性
——（此处应有掌声 👏👏👏）

最后，我们来看一下号称史上第一款、目前仅此一家内置 [Let’s
Encrypt](https://letsencrypt.org/) 支持的 Web 服务器，是如何 XXX 的。

我目前只申请了 <https://gopm.io> 的资格，所以就拿这个来举例。

我感觉我已经快没词了，直接上配置文件吧：

    https://gopm.io {
        proxy / localhost:8084
    }

此时，你启动 caddy 需要带上参数
`-ca=https://acme-v01.api.letsencrypt.org`。

Caddy 会在启动后要求你输入申请的邮箱等信息（填错了直接把 `~/.caddy`
删了重启）。

然后，嗯，就没有然后了，网站配置 HTTPS 就是这么任性。

然而它就这么赤裸裸地抛弃了传统的 HTTPS？显然没，我也还有用 StartSSL
证书的站点。配置如下：

    https://gogs.io {
        tls /path/to/gogs.io.unified.crt /path/to/gogs.io-decrypted.key
        proxy / localhost:5555
    }

（全文完）

最后奉上已经捂出汗的 Systemd 配置文件（不用的请绕道！）以供参考：

    [Unit]
    Description=Caddy Server
    After=syslog.target
    After=network.target

    [Service]
    User=root
    Group=root
    LimitNOFILE=64000
    ExecStart=/usr/local/bin/caddy --conf=/path/to/Caddyfile -ca=https://acme-v01.api.letsencrypt.org
    Restart=always

    [Install]
    WantedBy=multi-user.target

对性能感兴趣的朋友可以直接去 [FAQ](https://caddyserver.com/docs/faq)
页面查看和 NGINX 以及 Apache 的对比。

