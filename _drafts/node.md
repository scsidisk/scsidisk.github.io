node.js express 运行环境 NODE_ENV
2014年09月05日 ⁄ 综合 ⁄ 共 766字	⁄ 字号 小 中 大 ⁄ 评论关闭
Express支持多工作环境，比如生产环境 和开发环境 等。开发者可以使用configure() 方法根据当前环境的需要进行设置，当configure() 没有传入环境名称时，它会在各环境之前被调用（一回注：相当于被各个明确环境所共享）。
下面的示例我们只抛出异常（dumpException ），并且在开发模式 对异常堆栈的输出做出响应，但是不论对开发或者生产环境我们都使用了methodOverride 和bodyParser 。
// 定义共享环境
app.configure(function(){
    app.use(express.methodOverride());
    app.use(express.bodyParser());
    app.use(app.router);
});

// 定义开发环境
app.configure('development', function(){
    app.use(express.static(__dirname + '/public'));
    app.use(express.errorHandler({ dumpExceptions: true, showStack: true }));
});

// 定义生产环境
app.configure('production', function(){
    var oneYear = 31557600000;
    app.use(express.static(__dirname + '/public', { maxAge: oneYear }));
    app.use(express.errorHandler());
});





要修改环境，可以通过设置NODE_ENV

环境变量来实现，例如：
在linux下：export NODE_ENV=production
然后node app.js



这很重要

，因为许多的缓存机制只有在生产环境才会启用



一种简单的生产环境部署Node.js程序方法
 发布于 7个月前  作者 leizongmin  1676 次浏览
最近在部署Node.js程序时，写了段简单的脚本，发觉还挺简单的，忍不住想与大家分享。

配置文件

首先，本地测试环境和生产环境的数据库连接这些配置信息是不一样的，需要将其分开为两个文件存储
到config目录下，比如：

开发环境配置文件config/development.js：

module.exports = {
  port:  3001,
  mysql: {
    user: 'root'
  }
};
生产环境配置文件config/production.js:

module.exports = {
  port: 80,
  mysql: {
    user: 'myapp',
    password: '2zbonsjzl305vkh3'
  }
};
另外还要建立一个程序自动载入相应环境的配置，文件config/index.js：

var path = require('path');

// 通过NODE_ENV来设置环境变量，如果没有指定则默认为生产环境
var env = process.env.NODE_ENV || 'production';
env = env.toLowerCase();

// 载入配置文件
var file = path.resolve(__dirname, env);
try {
  var config = module.exports = require(file);
  console.log('Load config: [%s] %s', env, file);
} catch (err) {
  console.error('Cannot load config: [%s] %s', env, file);
  throw err;
}
假设应用的入口文件是app.js，可通过以下方法载入配置：

var config = require('./config');

console.log('listen on port %s', config.port);
// 如果是开发环境，将输出 listen on port 3001
// 如果是生产环境，将输出 listen on port 80
本地开发测试

为了方便，我新建一个脚本文件run，代码如下：

export NODE_ENV=development
node app
要启动程序，直接在命令行下执行./run即可。

部署应用

新建部署脚本文件deploy，代码如下：

git reset --hard
git pull origin HEAD
npm install
pm2 stop myapp -f
pm2 start app.js -n myapp
此段代码会自动拉去git仓库中最新的一次提交的代码，并使用npm来安装package.json中列出的模块，
然后先停止之前已启动的应用实例，再启动。

为了方便传输代码到服务器端，需要将程序代码提交到一个私有的git仓库，首次在服务器端部署时，
需要先将代码clone到服务器端，比如：

git clone git[@github](/user/github).com:leizongmin/node-uc-server.git ~/myapp
应用在服务器端运行时使用pm2工具来管理进程，所以还需要先在服务器上安装此工具：

npm install pm2 -g
完成以上准备工作后，我们就可以通过deploy脚本来实现自动更新代码：

将本地修改提交到远程git仓库
登录服务器，进入~/myapp目录
执行./deploy
以上程序执行的环境为Linux，如果开发环境是Windows，需要将run文件改为以下代码：

set NODE_ENV=development
node app
扩展阅读