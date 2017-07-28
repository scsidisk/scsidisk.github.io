通过gitlab的webhook同步issue到trello
====================================

01月31日 2015 solution

工作中碰到的流程化问题，希望通过技术来解决

这周在工作中开始启用gitlab的issue，感觉还是不错的，但是唯一的小问题就是gitlab的issue不知道每个人正在进行中的任务

于是每次都要在两边加issue和card非常烦，所以大家就想着能不能同步到trello，发现github上面还是有很多做trello的api的项目

于是参考了trello的api文档，非常复杂的API建议没事别瞎玩

[Trello API Docs](https://trello.com/docs/)

Trello的api调用
---------------

### OAuth认证

我用了python的request框架号称最接近人类语言的

这边需要注意的是trello的oauth体系

首先需要你的key，从这边可以拿到你的key

[Developer API keys](https://trello.com/app-key)

然后用你的Key和app名字去拿token,以下是官方给的例子

    https://trello.com/1/authorize?response_type=token&key=6ea9956f523d009b3d74050a0d7c6b1b&
    return_url=https%3A%2F%2Ftrello.com&callback_method=postMessage&scope=read&expiration=1hour&
    name=Trello+Application+Key+Test

熟悉oauth2的同学一定知道他用的是token两步认证，直接拿到token，并且scope是read

如果你需要写看板，或者删除之类的，就是除了GET以外的所有操作，都需要加一个write权限!

在这里有个expiration，可以设置为never，从而在你的服务器端进行操作，但是比较危险，要保管好你的token

### 读写看板

gitlab当遇到issue提交会触发一个webhook，提交这样的数据到指定的url

    {
      "object_kind": "issue",
      "user": {
        "name": "Administrator",
        "username": "root",
        "avatar_url": "http://www.gravatar.com/avatar/e64c7d89f26bd1972efa854d13d7dd61?s=40\u0026d=identicon"
      },
      "object_attributes": {
        "id": 301,
        "title": "New API: create/update/delete file",
        "assignee_id": 51,
        "author_id": 51,
        "project_id": 14,
        "created_at": "2013-12-03T17:15:43Z",
        "updated_at": "2013-12-03T17:15:43Z",
        "position": 0,
        "branch_name": null,
        "description": "Create new API for manipulations with repository",
        "milestone_id": null,
        "state": "opened",
        "iid": 23,
        "url": "http://example.com/diaspora/issues/23",
        "action": "open"
      }
    }

这是请求的头，即request.body，我们用python的bottle微框架来处理请求

    @route('/hello/<name>')
    def index(name):
        return template('<b>Hello </b>!', name=name)

    @post('/issue')
    def issue():
        issue = request.json
        if issue['object_attributes']['action'] == 'open':
            trello.writeCardToList("[ ISSUE %s ] %s" % (issue['object_attributes']['iid'],issue['object_attributes']['title']),desc % (issue['object_attributes']['iid'],issue['object_attributes']['url']))
        return "OK"

我们的需求主要是当issue的动作是打开时，提交看板的issue号码到trello，并且附带一个链接，指向相应的看板

python程序部署
---------------

nginx

    location /issue {
        include uwsgi_params;
        uwsgi_pass 127.0.0.1:8080;
    }
    location /hello {
        include uwsgi_params;
        uwsgi_pass 127.0.0.1:8080;
    }

uwsgi

类似于Rack的中间件

    [uwsgi]
    plugins = python
    socket = 127.0.0.1:8080
    logto = /var/log/bottle.log
    home = /home/megrez/gitlab2trello
    chdir = /home/megrez/gitlab2trello
    python-path = /home/megrez/gitlab2trello
    virtualenv = /home/megrez/gitlab2trello
    module = server

重启服务

    megrez@whosv-production-0:~/gitlab2trello$ sudo service uwsgi restart ```

virtualenv

建立python的虚拟环境，保证所有依赖被正确安装

    megrez@whosv-production-0:~/gitlab2trello$ virtualenv /home/megrez/gitlab2trello
    // 这会在该目录下生成一个bin,lib,local这几个文件夹
    megrez@whosv-production-0:~/gitlab2trello$ source bin/activate
    (gitlab2trello)megrez@whosv-production-0:~/gitlab2trello$ pip install -r requirements.txt

Bottle

    #!/usr/bin/env python
    ...
    if __name__ == "__main__":
        run(host='localhost', port=8080)
    else:
        application = default_app()
