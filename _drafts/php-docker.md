PHP开发环境 - Docker(虚拟机环境)
=============================

[Docker](http://www.docker.com/) 是个很有意思的开源项目，它彻底释放了虚拟化的威力，极大降低了云计算资源供应的成本，同时让应用的部署、测试和分发都变得前所未有的高效和轻松！

环境
----

- Mac OS X 10.10

提前准备知识
----------

[Docker入门学习](http://dockerpool.com/static/books/docker_practice/index.html)

[国内docker镜像加速服务](https://www.daocloud.io/) （免费的）

安装Docker
---------

这个Docker的发行版本不建议使用 Boot2Docker 命令行，建议使用 Docker Machine。使用  DockerToolbox 安装 Docker Machine 跟安装其他 Docker 工具一样。

### 安装 Docker Toolbox

    $ brew install caskroom/cask/brew-cask
    $ brew cask install dockertoolbox
    $ docker version

看到版本信息，表示 mac os x 的 docker 环境安装成功

虚拟机操作
---------

### 创建虚拟机

    $ docker-machine create --driver virtualbox default
    $ docker-machine start default
    $ docker-machine stop default

这里在 VirtualBox 里，创建了一个名称为 default 的虚拟机。

机器配置文件，在~/.docker/machine/machines/default

之后，你可以在命令行里，使用 docker-machine 去启动、停止、查询和其他管理虚拟机。

#### 列出你的可用机器

    $ docker-machine ls

如果你以前安装了被弃用的Boot2Docker 应用或运行Docker 快速启动终端，你可能还有一个 dev 的虚拟机。

#### 获取你的新虚拟机的环境命令

    $ docker-machine env default

#### 连接你的shell到 default 机器

    $ eval "$(docker-machine env default)"

#### 登入虚拟机

    $ docker-machine ssh default

#### 运行 hello-world 容器去校验你的设置

    $ docker run hello-world

#### 从Boot2Docker迁移

如果你之前使用 Boot2Docker，你拥有一个Docker boot2docker-vm 虚拟机，在你的本地系统上。如果你想让Docker Machine 去管理旧的虚拟机，你可以迁移它。

    $ docker-machine create -d virtualbox --virtualbox-import-boot2docker-vm boot2docker-vm docker-vm

使用 docker-machine 命令发起虚拟的迁移通信

容器操作
-------

下面已 nginx 为例，尝试使用 Docker 运行。

### 下载境像

搜索你需要的镜像，很多镜像已经创建，通过stars 和里面的介绍，可以判断受欢迎的程度

    $ docker search nginx
    $ docker pull nginx

当然，你也可以在启动时，配置指定的镜像仓库 --registry-mirror

### 启动容器

> 在 Mac 下面，路径要是本机磁盘，不要使用`软链接`或`外置存储上面的目录`。

    $ mkdir $HOME/website
    $ echo "It works." > $HOME/website/index.html
    $ docker run -itd -p 2023:22 -p 8080:80 \
    -v $HOME/website:/usr/share/nginx/html \
    --name nginx-test nginx


参数说明

    -itd: 已守护进程方式运行
    -p: 端品映射， 80（ 开发机端口 ）: 8080 ( 容器端口）
    -v: 目录映射(不能是软链接)，$HOME/website (开发机目录): /usr/share/nginx/html （容器目录）
    --name: 容器名称或标签，可以在启、停、删除容器时、与其它容器进行连接时使用
    nginx 镜像名称

默认容器端口说明：

    8080：nginx服务端口
    22：ssh服务端口

#### 获取 website 容器的端口。

    $ docker port website
    22/tcp -> 0.0.0.0:2023
    80/tcp -> 0.0.0.0:8080

#### 访问网站

因为宿主机器是我们上面创建的 virtualbox 虚拟机, 所以 nginx 网站需要在虚拟机上面查看。

    $ docker-machine ip default
    192.168.1.100

在浏览器里面访问 http://192.168.1.100:8080 即可看到 `It works.` 。

#### 启动，停止并删除你运行中的 mysite 容器。

    $ docker start nginx-test
    $ docker stop nginx-test
    $ docker rm nginx-test

### 容器本地存储位置

在虚拟机里面 /var/lib/docker 目录存储容器.

- `/var/lib/docker/{driver-name}` 包含镜像内容的驱动
- `/var/lib/docker/graph/<id>` 包含镜像的元数据，json结构.

Docker常用命令说明
----------------

### 镜像

    # 查看镜像列表
    $ docker images
    # 拉取镜像
    $ docker pull 仓库地址：ip/tag名称
    # 推送镜像
    $ docker push 仓库地址：ip/tag名称
    # 删除镜像
    $ docker rmi <REPOSITORY>

### 容器

    # 查看容器列表
    $ docker ps -a
    # 创建容器
    $ docker run -p -v -name 镜像名称 运行程序
    # 删除容器
    $ docker rm -f 镜像名称或唯一hash值
    # 停止、启动容器
    $ docker stop 容器名称
    $ docker start 容器名称

私有仓库
-------

公司会有自己的镜像，使用 Docker Hub 这样的公共仓库可能不方便，我们可以创建一个本地仓库供私人使用。

使用官方提供的工具 docker-registry ，可以方便地构建私有的镜像仓库。

> 如果切换无线和有线，需要重新启动虚拟机，让虚拟机使用新的网络才行。

### 安装运行 docker-registry

    $ docker --tls=false run -d -p 5000:5000 \
    --restart=always \
    --name registry \
    registry:2

这将使用官方的 registry 镜像来启动本地的私有仓库。 用户可以通过指定参数来配置私有仓库位置，例如配置镜像存储到 Amazon S3 服务。

还可以指定本地路径（如 /home/user/registry-conf ）下的配置文件。

    $ docker run -d -p 5000:5000 \
    -v /home/user/registry-conf:/registry-conf \
    -v /opt/data/registry:/tmp/registry
    -e DOCKER_REGISTRY_CONFIG=/registry-conf/config.yml \
    registry

默认情况下，仓库会被创建在容器的 /tmp/registry 下。可以通过 -v 参数来将镜像文件存放在本地的 /opt/data/registry 目录。

对于 Ubuntu 或 CentOS 等发行版，可以直接通过源安装。

    Ubuntu
    $ sudo apt-get install -y build-essential python-dev libevent-dev python-pip liblzma-dev
    $ sudo pip install docker-registry
    CentOS
    $ sudo yum install -y python-devel libevent-devel python-pip gcc xz-devel
    $ sudo python-pip install docker-registry

使用 curl 访问本地的 5000 端口，看不到输出信息，但是没有报错，运行成功。

    $ curl localhost:5000

在私有仓库上传、下载、搜索镜像
--------------------------

创建好私有仓库之后，就可以使用 docker tag 来标记一个镜像，然后推送它到仓库，别的机器上就可以下载下来了。例如私有仓库地址为 localhost:5000。

    ## 先在本机查看已有的镜像。
    $ sudo docker images
    REPOSITORY     TAG       IMAGE ID        CREATED         VIRTUAL SIZE
    ubuntu         latest    c4bea91afef3    6 weeks ago     187.9 MB
    $ docker tag --help
    $ docker tag ubuntu localhost:5000/ubuntu
    $ docker images
    ## 可以发现名为 `localhost:5000/ubuntu` 的 image
    $ docker push localhost:5000/ubuntu
    The push refers to a repository [localhost:5000/ubuntu] (len: 1)
    c4bea91afef3: Pushed
    d9e545b90db8: Pushed
    4cdc0cbc1936: Pushed
    fcee8bcfe180: Pushed
    latest: digest: sha256:aeb4d9f0ce87088b8b1f54cd07c8463036529c23ac8edc30db09e72af102d6b2 size: 6800
    ## 用 curl 查看仓库中的镜像。
    $ curl -v -X GET http://192.168.99.100:5000/v2/_catalog
    ## 查看镜像标签列表
    $ curl -v -X GET http://192.168.99.100:5000/v2/ubuntu/tags/list
    ## 使用本地镜像
    $ docker run localhost:5000/ubuntu

说明已经从本地上传上去，这种方法只适用与个人测试和调试， 但是如果使用非 localhost 进行下载的时候会要求使用 ssl 方式。提示错误

registry 镜像应用到生成环境
------------------------

    ## 现在可以到另外一台机器去下载这个镜像。
    $ docker pull 192.168.7.26:5000/test
Pulling repository 192.168.7.26:5000/test
ba5877dc9bec: Download complete
511136ea3c5a: Download complete
9bad880da3d2: Download complete
25f11f5fb0cb: Download complete
ebc34468f71d: Download complete
2318d26665ef: Download complete
$ sudo docker images
REPOSITORY                         TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
192.168.7.26:5000/test             latest              ba5877dc9bec        6 weeks ago         192.7 MB
可以使用 这个脚本 批量上传本地的镜像到注册服务器中，默认是本地注册服务器 127.0.0.1:5000。例如：
$ wget https://github.com/yeasy/docker_practice/raw/master/_local/push_images.sh; sudo chmod a+x push_images.sh
$ ./push_images.sh ubuntu:latest centos:centos7
The registry server is 127.0.0.1
Uploading ubuntu:latest...
The push refers to a repository [127.0.0.1:5000/ubuntu] (len: 1)
Sending image list
Pushing repository 127.0.0.1:5000/ubuntu (1 tags)
Image 511136ea3c5a already pushed, skipping
Image bfb8b5a2ad34 already pushed, skipping
Image c1f3bdbd8355 already pushed, skipping
Image 897578f527ae already pushed, skipping
Image 9387bcc9826e already pushed, skipping
Image 809ed259f845 already pushed, skipping
Image 96864a7d2df3 already pushed, skipping
Pushing tag for rev [96864a7d2df3] on {http://127.0.0.1:5000/v1/repositories/ubuntu/tags/latest}
Untagged: 127.0.0.1:5000/ubuntu:latest
Done
Uploading centos:centos7...
The push refers to a repository [127.0.0.1:5000/centos] (len: 1)
Sending image list
Pushing repository 127.0.0.1:5000/centos (1 tags)
Image 511136ea3c5a already pushed, skipping
34e94e67e63a: Image successfully pushed
70214e5d0a90: Image successfully pushed
Pushing tag for rev [70214e5d0a90] on {http://127.0.0.1:5000/v1/repositories/centos/tags/centos7}
Untagged: 127.0.0.1:5000/centos:centos7
Done

