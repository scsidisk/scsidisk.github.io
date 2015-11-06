# Express 4 使用指南

## 安装 node (Mac OS X 10)

```
brew install node
```

## 安装 Express 4

```
$ npm install -g express-generator
```

## 生成项目

如果你想生成一个支持ejs, Stylus的应用程序，只需要简单的执行下面的命令

```
$ express --sessions --css stylus --ejs myapp
$ cd myapp
$ npm install
$ DEBUG=joodoc:* ./bin/www
```

即可运行起来了！

这些就是一个简单的应用程序创建和运行的所有步骤。 记住Express没有限定任何的目录结构，这只是一个方便你工作的基本结构。 如果你想得到更多怎么组织目录结构选择，可以查看github上的示例。

## 热加载

```
$ npm install node-supervisor -g
$ node-supervisor ./bin/www
```

## 热部署

```
$ npm install forever -g
$ forever start ./bin/www
```

## i18n

```
$ npm install i18n
```

app.js 中添加

```
var i18n = require("i18n");
...
app.use(i18n.init);
...
i18n.configure({
    locales:['en', 'zh_cn'],
    directory: __dirname + '/locales',
    defaultLocale: 'en',
    cookie: 'lang'
});
```

模板文件 index.html 中使用

```
<%= __('Hello') %>
```

即可在 locales 目录中看到相应的文件，翻译后面的值，刷新即可看到翻译效果。

## 错误处理

错误处理的中间件和普通的中间件定义是一样的， 只是它必须有4个形参，这是它的形式： (err, req, res, next):

```
app.use(function(err, req, res, next){
  console.error(err.stack);
  res.send(500, 'Something broke!');
});
```

一般来说非强制性的错误处理一般被定义在最后，下面的代码展示的就是放在别的 app.use() 之后：

```
app.use(express.bodyParser());
app.use(express.methodOverride());
app.use(app.router);
app.use(function(err, req, res, next){
  // logic
});
```

在这些中间件里的响应是可以任意定义的。只要你喜欢，你可以返回任意的内容，譬如HTML页面, 一个简单的消息，或者一个JSON字符串。

对于一些组织或者更高层次的框架，你可能会像定义普通的中间件一样定义一些错误处理的中间件。 假设你想定义一个中间件区别对待通过XHR和其它请求的错误处理，你可以这么做：

```
app.use(express.bodyParser());
app.use(express.methodOverride());
app.use(app.router);
app.use(logErrors);
app.use(clientErrorHandler);
app.use(errorHandler);
```

通常logErrors用来纪录诸如stderr, loggly, 或者类似服务的错误信息：

```
function logErrors(err, req, res, next) {
  console.error(err.stack);
  next(err);
}
```

clientErrorHandler 定义如下，注意错误非常明确的向后传递了。

```
function clientErrorHandler(err, req, res, next) {
  if (req.xhr) {
    res.send(500, { error: 'Something blew up!' });
  } else {
    next(err);
  }
}
```

下面的errorHandler "捕获所有" 的异常， 定义为:

```
function errorHandler(err, req, res, next) {
  res.status(500);
  res.render('error', { error: err });
}
```

## 在线用户计数

这一小节我们讲解一个小而全的应用程序，它通过Redis记录在线用户数。 首先你需要创建一个package.json 文件，包含两个依赖, 一个是redis 客户端，另一个是Express。 另外需要确认你安装了redis, 可以能过执行$ redis-server来确认：

```
{
  "name": "app",
  "version": "0.0.1",
  "dependencies": {
    "express": "3.x",
    "redis": "*"
  }
}
```

接下来你需要你创建一个应用程序，和一个redis连接：

```
var express = require('express');
var redis = require('redis');
var db = redis.createClient();
var app = express();
```

接下来是纪录用户在线的中间件。 这里我们使用sorted sets, 它的一个好处是我们可以查询最近N毫秒内在线的用户。 我们通过传入一个时间戳来当作成员的"score"。 注意我们使用 User-Agent 作为一个标识用户的id。

```
app.use(function(req, res, next){
  var ua = req.headers['user-agent'];
  db.zadd('online', Date.now(), ua, next);
});
```

下一个中间件是通过zrevrangebyscore来查询上一分钟在线用户。 我们将能得到从当前时间算起在60,000毫秒内活跃的用户。

```
app.use(function(req, res, next){
  var min = 60 * 1000;
  var ago = Date.now() - min;
  db.zrevrangebyscore('online', '+inf', ago, function(err, users){
    if (err) return next(err);
    req.online = users;
    next();
  });
});
```

最后我们来使用它，绑定到一个端口！这些就是这个程序的一切了，在不同的浏览器里访问这个应用程序，你会看到计数的增长。

```
app.get('/', function(req, res){
  res.send(req.online.length + ' users online');
});

app.listen(3000);
```

## 给Express加一层代理

在Express的前端使用一个反向代理，比如 Varnish 或者 Nginx是非常常见的,它不需要额外的配置。 在通过app.enable('trust proxy')激活了"trust proxy" 设置后， Express 就会知道它在一个代理的后面，X-Forwarded-* 必须被信任， 通常情况下这些头是很容易被伪装的。

使用了这个设置后会有一些很棒的小变化。 首先由代理设置的X-Forwarded-Proto 会告诉程序它是https 还是http 。 这个值会影响req.protocol.

第二个变化是 req.ip 和 req.ips 的值会被X-Forwarded-For列表里的地址取代。

## 调试 Express

Express使用 debug 模块 来输出信息。如果想看到这些信息，可以在运行你的程序时设置 DEBUG 环境变量为 express:* ,调试信息会输出在终端里 。

```
$ DEBUG=express:* node app.js
```

使用上面的方式运行 hello world 的例子，将会输出下面的内容

```
express:application booting in development mode +0ms
express:router defined get /hello.txt +0ms
express:router defined get /hello.txt +1ms
```

获取更多关于 debug的信息，可以查看 debug 文档


## 热部署

forever start ./bin/www
forever stop ./bin/www