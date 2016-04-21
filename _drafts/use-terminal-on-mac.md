
优化mac自带的ssh

一：设置自动登录和连接共享

Host *
    ControlMaster auto
    ControlPath ~/.ssh/%h-%p-%r
    ControlPersist yes
    Host short_name
    hostname relay01.xxx.com
    user xxx

    功能 ：
     之前：ssh xxx@relay01
     输入 pass+token

     现在：ssh baidu 直接ok
     （具体可以查询.ssh config 设置）

     参考地址：ssh_config 配置详细介绍
分析：
1

2

ControlMaster auto
ControlPath

第一句话是多条连接共享【就是同样连接第二次就会很快，关键是不需要数据token】
3
 controlPersist 3h (设置连接时间 yes表示永久，除非网断了)

 下面的是一个快捷键别名，节省输入时间。

二：记忆密码

参考文章：如何优雅地连接ssh

三：中间条状【例如一般公司开发，都需要先登录a机器，再跳转】
使用
proxycommand ssh xxx 进行下一步

如此配置完成了，感觉基本上跳转和使用也挺方便（和securecrt感觉差不多）

同时，如果觉得机器名称多，可以添加一个快捷记忆
【小技巧】一般我都配置在全局变量里面，直接echo $xx 就是这个的简称了


