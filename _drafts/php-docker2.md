Docker部署私有仓库
=================

今天和大家聊聊Docker的私有仓库。

前段时间啊在CentOS6.x上玩Docker的私有仓库，由于https认证的原因，一直没有能解决，最后听群上的一朋友说，换成CentOS
7试试，也别说，最后实验成功啦！

所以我建议朋友在玩docker的私有仓库的时候，也能现在CentOS7.x系统上玩，确定对整个过程熟悉后，然后换成你熟悉的6.x的系统，这样也是一个循循渐进的过程吧！

一、准备

1、地址规划

```
Docker私有仓库地址：192.168.0.109
Docker客户端地址：192.168.0.110
```

2、激活网卡

```
# vim /etc/sysconfig/network-scripts/ifcfg-eno16777728
修改此行
ONBOOT=yes
# /etc/init.d/network restart
```

3、关闭本地防火墙并设置开机不自启动

```
# systemctl stop firewalld.service
# systemctl disable firewalld.service
```

4、关闭本地selinux防火墙

```
# vi /etc/sysconfig/selinux 
SELINUX=disabled
# setenforce 0
```

5、安装ifconfig工具

```
# yum install net-tools
```

1、安装docker

```
# yum install docker
# yum upgrade device-mapper-libs
# service docker start
# chkconfig docker on
```

2、本地私有仓库registry

```
[root@localhost ~]# docker pull registry
Trying to pull repository docker.io/registry ...
24dd746e9b9f: Download complete 
706766fe1019: Download complete 
a62a42e77c9c: Download complete 
2c014f14d3d9: Download complete 
b7cf8f0d9e82: Download complete 
d0b170ebeeab: Download complete 
171efc310edf: Download complete 
522ed614b07a: Download complete 
605ed9113b86: Download complete 
22b93b23ebb9: Download complete 
2ac557a88fda: Download complete 
1f3b4c532640: Download complete 
27ebaac643a7: Download complete 
ce630195cb45: Download complete 
Status: Downloaded newer image for docker.io/registry:latest
[root@localhost ~]# docker images
REPOSITORY           TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
docker.io/registry   latest              24dd746e9b9f        3 days ago          413.8 MB
```

3、基于私有仓库镜像运行容器

```
[root@localhost ~]# docker run -d -p 5000:5000 -v /opt/data/registry:/tmp/registry docker.io/registry
bb2c0d442df94e281479332c2608ef144f378e71743c5410e36b80c465771a95
[root@localhost ~]# docker ps -a
CONTAINER ID        IMAGE                       COMMAND             CREATED             STATUS              PORTS                    NAMES
bb2c0d442df9        docker.io/registry:latest   "docker-registry"   10 seconds ago      Up 7 seconds        0.0.0.0:5000->5000/tcp   serene_hopper
```

4、访问私有仓库

```
[root@localhost ~]# curl 127.0.0.1:5000/v1/search
 //私有仓库为空，没有提交新镜像到仓库中
```

5、从Docker
Hub上下载一个ssh镜像

```
[root@localhost ~]# docker search -s 10 ssh
NAME                              DESCRIPTION   STARS     OFFICIAL   AUTOMATED
docker.io: docker.io/fedora/ssh                 18                   [OK]
[root@localhost ~]# docker pull fedora/ssh
Trying to pull repository docker.io/fedora/ssh ...
2aeb2b6d9705: Download complete 
511136ea3c5a: Download complete 
00a0c78eeb6d: Download complete 
834629358fe2: Download complete 
571e8a51403c: Download complete 
87d5d42e693c: Download complete 
92b5ef05fe68: Download complete 
92d3910dc33c: Download complete 
cf2e9fa11368: Download complete 
Status: Downloaded newer image for docker.io/fedora/ssh:latest
[root@localhost ~]# docker images
REPOSITORY             TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
docker.io/registry     latest              24dd746e9b9f        3 days ago          413.8 MB
docker.io/fedora/ssh   latest              2aeb2b6d9705        9 days ago          254.4 MB
```

6、创建镜像链接或为基础镜像打个标签

```
[root@localhost ~]# docker tag docker.io/fedora/ssh 127.0.0.1:5000/ssh
[root@localhost ~]# docker images
REPOSITORY             TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
docker.io/registry     latest              24dd746e9b9f        3 days ago          413.8 MB
docker.io/fedora/ssh   latest              2aeb2b6d9705        9 days ago          254.4 MB
127.0.0.1:5000/ssh     latest              2aeb2b6d9705        9 days ago          254.4 MB
```

7、修改Docker配置文件制定私有仓库url

```
[root@localhost ~]# vim /etc/sysconfig/docker
修改此行
OPTIONS='--selinux-enabled --insecure-registry 192.168.0.109:5000'
[root@localhost ~]# service docker restart
Redirecting to /bin/systemctl restart  docker.service
```

8、提交镜像到本地私有仓库中

```
[root@localhost ~]# docker push 127.0.0.1:5000/ssh
The push refers to a repository [127.0.0.1:5000/ssh] (len: 1)
Sending image list
Pushing repository 127.0.0.1:5000/ssh (1 tags)
511136ea3c5a: Image successfully pushed 
00a0c78eeb6d: Image successfully pushed 
834629358fe2: Image successfully pushed 
571e8a51403c: Image successfully pushed 
87d5d42e693c: Image successfully pushed 
92b5ef05fe68: Image successfully pushed 
92d3910dc33c: Image successfully pushed 
cf2e9fa11368: Image successfully pushed 
2aeb2b6d9705: Image successfully pushed 
Pushing tag for rev [2aeb2b6d9705] on 
```

9、查看私有仓库是否存在对应的镜像

```
[root@localhost ~]# curl 127.0.0.1:5000/v1/search

```

10、查看镜像的存储目录和文件

```
[root@localhost ~]# tree /opt/data/registry/repositories/
/opt/data/registry/repositories/
└── library
    └── ssh
        ├── _index_images
        ├── json
        ├── tag_latest
        └── taglatest_json

2 directories, 4 files
```


三、从私有仓库中下载已有的镜像

1、登陆另外一台Docker客户端

```
[root@localhost ~]# ssh root@192.168.0.110
The authenticity of host '192.168.0.110 (192.168.0.110)' can't be established.
ECDSA key fingerprint is 5b:81:4b:66:d6:dd:48:16:9f:85:58:72:21:bd:ba:39.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.0.110' (ECDSA) to the list of known hosts.
root@192.168.0.110's password: 
Last login: Sun Apr 26 14:39:51 2015 from 192.168.0.103
```

2、修改Docker配置文件

```
[root@localhost ~]# vim /etc/sysconfig/docker
修改此行
OPTIONS='--selinux-enabled --insecure-registry 192.168.0.109:5000'        //添加私有仓库地址
[root@localhost ~]# service docker restart
Redirecting to /bin/systemctl restart  docker.service
```

3、从私有仓库中下载已有的镜像

```
[root@localhost ~]# docker pull 192.168.0.109:5000/ssh
Trying to pull repository 192.168.0.109:5000/ssh ...
2aeb2b6d9705: Download complete 
511136ea3c5a: Download complete 
00a0c78eeb6d: Download complete 
834629358fe2: Download complete 
571e8a51403c: Download complete 
87d5d42e693c: Download complete 
92b5ef05fe68: Download complete 
92d3910dc33c: Download complete 
cf2e9fa11368: Download complete 
Status: Downloaded newer image for 192.168.0.109:5000/ssh:latest
[root@localhost ~]# docker images
REPOSITORY               TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
192.168.0.109:5000/ssh   latest              2aeb2b6d9705        9 days ago          254.4 MB
```

本文出自 “[郑彦生](http://467754239.blog.51cto.com)” 博客






