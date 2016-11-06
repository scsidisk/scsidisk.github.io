基于gitlab搭建CI环境
===================


### 前置条件

务必安装gitlab，且保证服务可用。如若安装出现问题，请参阅安装方式。

### 安装gitlab-ci-runnter

安装流程参考 https://gitlab.com/gitlab-org/gitlab-ci-multi-runner/blob/master/docs/install/linux-manually.md，如果网络允许，可以采用https://gitlab.com/gitlab-org/gitlab-ci-multi-runner#installation更加自动的方式。

如果使用 Docker runner, 需要提前安装:

    curl -sSL https://get.docker.com/ | sh

安装 GitLab's 官方代码库

    # For Debian/Ubuntu
    curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-ci-multi-runner/script.deb.sh | sudo bash

    # For CentOS
    curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-ci-multi-runner/script.rpm.sh | sudo bash

安装 gitlab-ci-multi-runner:

    # For Debian/Ubuntu
    sudo apt-get install gitlab-ci-multi-runner

    # For CentOS
    sudo yum install gitlab-ci-multi-runner

注册 runner:

    sudo gitlab-ci-multi-runner register

    Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/ci )
    https://gitlab.com/ci ## ci的位置
    Please enter the gitlab-ci token for this runner
    xxx ## ci 里面显示的 gitlab-ci token
    Please enter the gitlab-ci description for this runner
    my-runner ## 描述
    INFO[0034] fcf5c619 Registering runner... succeeded
    Please enter the executor: shell, docker, docker-ssh, ssh?
    docker ## 执行类型
    Please enter the Docker image (eg. ruby:2.1):
    ruby:2.1 ## 如果是 docker 会要求 docker 的映像
    INFO[0037] Runner registered successfully. Feel free to start it, but if it's
    running already the config should be automatically reloaded!

The runner should be started already and you are ready to build your projects!
Make sure that you read the FAQ section which describes some of the most common problems with GitLab Runner.

更新 runner

Simply execute to install latest version:

    # For Debian/Ubuntu
    sudo apt-get update
    sudo apt-get install gitlab-ci-multi-runner

    # For CentOS
    sudo yum update
    sudo yum install gitlab-ci-multi-runner


### 配置gitlab-ci-runner

进入CI项目，进入Runners标签页面，可以看到CI的url和token，这2个值是待会用命令注册runner时所需要的。

    gitlab-ci-multi-runner register

首先配置各项基本信息，gitlab-ci coordinator URL一般就是gitlab host+"/ci"。

token是另外一个重要信息，runner有share runner跟specific runner。share runner可以转变为specific runner，但不可逆转。

在管理员面板可以找到share runner的token，如下图所示：

![](/static/images/2016/05/gitlab-runner-01.png)

在项目配置下可以找到specific runner的token，如下图所示：

![](/static/images/2016/05/gitlab-runner-02.png)

tag相当于为share runner分组，方便管理，所以最好不要随意输入。

注册完成后，打开runner的配置文件：`vi /etc/gitlab-runner/config.toml`， 可以看到配置文件里面增加了刚才注册的相关信息，更多参数的信息可以看官方文档。


然后，启动runner即可：

    gitlab-ci-multi-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
    gitlab-ci-multi-runner start

然后在管理员面板便能找到配置好的runner信息。

### 安装服务

执行命令

    gitlab-ci-multi-runner install -n "服务名"，

后面的服务名是自己定义的名称，用来后面启动命名使用，与其相对的命令是uninstall。

启动服务，执行命令gitlab-ci-multi-runner start -n "服务名"，与其相类似的命令有stop和restart。

### 验证runner

    gitlab-ci-multi-runner verify

可以看到runner的运行情况。

### gitlab-ci.yaml文件

配置好了runner，要让CI跑起来，还需要在项目根目录放一个.gitlab-ci.yaml文件，在这个文件里面可以定制CI的任务，下面是简单的示例文件，更多的用法可以看官方文档。

    before_script:
      - bundle_install
    job1:
      script:
        - execute-script-for-job1

### 使用gitlab-ci

只需在项目下添加.gitlab-ci.yml文件即可，配置内容参考http://doc.gitlab.com/ce/ci/yaml/README.html。具体的测试与部署流程不尽相同，所以此处并不做过多讨论，仅给出简单示例。

![](/static/images/2016/05/gitlab-runner-03.png)

### 常见问题

- 为么不选用经过检验的Jenkins？
jenkins插件丰富，功能强大，与各种代码管理工具都可以搭配。如果已有Jenkins经验，建议继续使用。如果已经确定使用gitlab作为代码库管理，则推荐使用gitlab-ci。毕竟集成度更加高，使用更加方便。

- 构建任务一直处于pending状态，无法响应?
处理方案：优先检查runner状态，然后检查tag是否匹配。

        # 检测当前runner运行状态
        gitlab-ci-multi-runner verify
        # 检测runner服务状态
        gitlab-ci-multi-runner status

- 运行时环境变量不匹配
处理方案： runner运行以gitlab-runner用户执行命令，所以跟其他用户的bash配置不尽相同，切换用户后修改即可。