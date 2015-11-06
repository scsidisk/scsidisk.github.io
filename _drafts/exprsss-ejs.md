开始
在确认已经安装了node之后(下载), 在你的机器上创建一个目录，让我们来开始你的第一个应用程序吧

$ mkdir hello-world
在这个目录中你首先得定义一下你的应用程序“包”文件，它和其它的node程序包是一样的。 你得在这个目录中创建一个package.json文件，在里面express作为一个依赖。 你也可以使用 npm info express version 来获取express最新的版本号， 最好使用最新的版本号而不是下面的3.x，这样新出的功能就不会让你感觉到奇怪了。

{
  "name": "hello-world",
  "description": "hello world test app",
  "version": "0.0.1",
  "private": true,
  "dependencies": {
    "express": "3.x"
  }
}
现在package.json文件已经准备好了，使用npm(1) 安装依赖， 这里的依赖仅仅是Express。

$ npm install
当npm完成后，Express 3.x 和它的依赖就安装到你的 ./node_modules 目录里了。 你可以通过 npm ls 来确认一下，它会把Express 和它的依赖展示成下面的树状结构。

$ npm ls
hello-world@0.0.1 /private/tmp
└─┬ express@3.0.0beta7
  ├── commander@0.6.1
  ├─┬ connect@2.3.9
  │ ├── bytes@0.1.0
  │ ├── cookie@0.0.4
  │ ├── crc@0.2.0
  │ ├── formidable@1.0.11
  │ └── qs@0.4.2
  ├── cookie@0.0.3
  ├── debug@0.7.0
  ├── fresh@0.1.0
  ├── methods@0.0.1
  ├── mkdirp@0.3.3
  ├── range-parser@0.0.4
  ├─┬ response-send@0.0.1
  │ └── crc@0.2.0
  └─┬ send@0.0.3
    └── mime@1.2.6
    
现在我们来写真正的代码了！创建一个名为app.js 或者 server.js的文件，叫什么看你个人喜好了。 载入express 然后使用代码 express()创建一个新的应用程序:

var express = require('express');
var app = express();
在这个应用程序实例里，你可以通过 app.VERB()定义路由，下面的例子是"GET /"返回 "Hello World" 字符串。 req 和 res 对象是和node原生提供给你的一致的，你也可以执行 res.pipe(), req.on('data', callback) 等任何事情在没有Express的情况下可以做的事情。

Express 给这些对象加了一个封装好的方法，比如 res.send(), 它会帮你设置Content-Length:

app.get('/hello.txt', function(req, res){
  res.send('Hello World');
});
现在我们通过执行 app.listen() 来绑定并监听连接。 它接受的参数和nodenet.Server#listen()的方法一致:

var server = app.listen(3000, function() {
    console.log('Listening on port %d', server.address().port);
});
使用express(1) 来生成一个应用程序
Express团队维护了一个可以快速生成项目模板的可执行文件，这里命名为 express(1). 如果你使用npm全局安装的express-generator, 在你的机器任何位置它都是可用的:

$ npm install -g express-generator
这个工具提供了一个非常简单的生成一个程序骨架的功能，但是它也有局限，比如它只支持很少的几个模板引擎。 而事实上Express几乎支持所有的为node所建的模板引擎。 使用 --help查看一下帮助:

Usage: express [options]

Options:

  -h, --help          输出帮助信息
  -V, --version       输出版本号
  -e, --ejs           添加 ejs 模板引擎支持 (默认为jade)
  -H, --hogan         添加 hogan.js模板引擎支持
  -c, --css   样式 <引擎> 支持 (less|stylus) (默认为css)
  -f, --force         强制在非空目录执行
如果你想生成一个支持Jade, Stylus的应用程序，只需要简单的执行下面的命令：

$ express --sessions --css stylus --ejs myapp

create : myapp
create : myapp/package.json
create : myapp/app.js
create : myapp/public
create : myapp/public/javascripts
create : myapp/public/images
create : myapp/public/stylesheets
create : myapp/public/stylesheets/style.styl
create : myapp/routes
create : myapp/routes/index.js
create : myapp/views
create : myapp/views/index.jade
create : myapp/views/layout.jade

install dependencies:
  $ cd myapp && npm install
  
run the app:
  $ DEBUG=myapp node app  
  
和其它node程序一样，你必须安装依赖：

$ cd myapp
$ npm install
然后让我们运行它吧！

$ node app
这些就是一个简单的应用程序创建和运行的所有步骤。 记住Express没有限定任何的目录结构，这只是一个方便你工作的基本结构。 如果你想得到更多怎么组织目录结构选择，可以查看github上的示例。

错误处理
错误处理的中间件和普通的中间件定义是一样的， 只是它必须有4个形参，这是它的形式： (err, req, res, next):

app.use(function(err, req, res, next){
  console.error(err.stack);
  res.send(500, 'Something broke!');
});
一般来说非强制性的错误处理一般被定义在最后，下面的代码展示的就是放在别的 app.use() 之后：

app.use(express.bodyParser());
app.use(express.methodOverride());
app.use(app.router);
app.use(function(err, req, res, next){
  // logic
});
在这些中间件里的响应是可以任意定义的。只要你喜欢，你可以返回任意的内容，譬如HTML页面, 一个简单的消息，或者一个JSON字符串。

对于一些组织或者更高层次的框架，你可能会像定义普通的中间件一样定义一些错误处理的中间件。 假设你想定义一个中间件区别对待通过XHR和其它请求的错误处理，你可以这么做：

app.use(express.bodyParser());
app.use(express.methodOverride());
app.use(app.router);
app.use(logErrors);
app.use(clientErrorHandler);
app.use(errorHandler);
通常logErrors用来纪录诸如stderr, loggly, 或者类似服务的错误信息：

function logErrors(err, req, res, next) {
  console.error(err.stack);
  next(err);
}
clientErrorHandler 定义如下，注意错误非常明确的向后传递了。

function clientErrorHandler(err, req, res, next) {
  if (req.xhr) {
    res.send(500, { error: 'Something blew up!' });
  } else {
    next(err);
  }
}
下面的errorHandler "捕获所有" 的异常， 定义为:

function errorHandler(err, req, res, next) {
  res.status(500);
  res.render('error', { error: err });
}
在线用户计数
这一小节我们讲解一个小而全的应用程序，它通过Redis记录在线用户数。 首先你需要创建一个package.json 文件，包含两个依赖, 一个是redis 客户端，另一个是Express。 另外需要确认你安装了redis, 可以能过执行$ redis-server来确认：

{
  "name": "app",
  "version": "0.0.1",
  "dependencies": {
    "express": "3.x",
    "redis": "*"
  }
}
接下来你需要你创建一个应用程序，和一个redis连接：

var express = require('express');
var redis = require('redis');
var db = redis.createClient();
var app = express();
接下来是纪录用户在线的中间件。 这里我们使用sorted sets, 它的一个好处是我们可以查询最近N毫秒内在线的用户。 我们通过传入一个时间戳来当作成员的"score"。 注意我们使用 User-Agent 作为一个标识用户的id。

app.use(function(req, res, next){
  var ua = req.headers['user-agent'];
  db.zadd('online', Date.now(), ua, next);
});
下一个中间件是通过zrevrangebyscore来查询上一分钟在线用户。 我们将能得到从当前时间算起在60,000毫秒内活跃的用户。

app.use(function(req, res, next){
  var min = 60 * 1000;
  var ago = Date.now() - min;
  db.zrevrangebyscore('online', '+inf', ago, function(err, users){
    if (err) return next(err);
    req.online = users;
    next();
  });
});
最后我们来使用它，绑定到一个端口！这些就是这个程序的一切了，在不同的浏览器里访问这个应用程序，你会看到计数的增长。

app.get('/', function(req, res){
  res.send(req.online.length + ' users online');
});

app.listen(3000);
给Express加一层代理
在Express的前端使用一个反向代理，比如 Varnish 或者 Nginx是非常常见的,它不需要额外的配置。 在通过app.enable('trust proxy')激活了"trust proxy" 设置后， Express 就会知道它在一个代理的后面，X-Forwarded-* 必须被信任， 通常情况下这些头是很容易被伪装的。

使用了这个设置后会有一些很棒的小变化。 首先由代理设置的X-Forwarded-Proto 会告诉程序它是https 还是http 。 这个值会影响req.protocol.

第二个变化是 req.ip 和 req.ips 的值会被X-Forwarded-For列表里的地址取代。

调试 Express
Express使用 debug 模块 来输出信息。如果想看到这些信息，可以在运行你的程序时设置 DEBUG 环境变量为 express:* ,调试信息会输出在终端里 。

$ DEBUG=express:* node app.js
使用上面的方式运行 hello world 的例子，将会输出下面的内容

express:application booting in development mode +0ms
express:router defined get /hello.txt +0ms
express:router defined get /hello.txt +1ms
获取更多关于 debug的信息，可以查看 debug 文档



https://www.npmjs.com/package/express-ejs-layouts

## express-ejs-layouts

Layout support for ejs in express.


Installation
npm install express-ejs-layouts
Usage
var express = require('express')
  , app = express()
  , expressLayouts = require('express-ejs-layouts')

app.set('view engine', 'ejs')
app.set('layout', 'myLayout') // defaults to 'layout'     

app.use(expressLayouts)
app.use(app.router)

app.get('/', function(req, res){
  res.render('aView', { layout: 'someSpecificLayout' })
})

app.listen(3000)
contentFor
A view

somebody
<%- contentFor('foo') %>
club
<%- contentFor('bar') %>
fight
With a layout

<%-bar%> <%-foo%>
<%-body%>
Renders

fight club
somebody
Script blocks extraction
If you like to place all the script blocks at the end, you can do it like this:

app.set("layout extractScripts", true)
A view

something<script>somejs<script>something
With a layout

...
<body>
<%- body %>
<%- script %>
</body>
Renders

...
<body>
somethingsomething
<script>somejs<script>
</body>
Enabling invididually:

req.render('view', { parseScript: true })
Optional sections
In a layout, you can have optional sections using defineContent: Unspecified section content defaults to ''.

1
<%-defineContent('a')%>
2
<%-defineContent('b')%>
3
with a view:

<%- contentFor('a') %>
1.5
will render:

1
1.5
2
3
Running tests
Clone the rep

make test

## 将模板页后缀换成.html

这是一个小技巧，看着.ejs的后缀总觉得不爽，使用如下方法，可以将模板文件的后缀换成我们习惯的.html。

1.在app.js的头上定义ejs:
var ejs = require('ejs');

2.注册html模板引擎：
app.engine('html',ejs.__express);

3.将模板引擎换成html:
app.set('view engine', 'html');

4.修改模板文件的后缀为.html。

好了，任务完成，可以运行观看效果了！