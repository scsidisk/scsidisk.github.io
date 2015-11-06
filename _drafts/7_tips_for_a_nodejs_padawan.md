面向Node.js新手的7个技巧
作者 / 张 金龙 标签： Node.js
原文：7 tips for a Node.js padawan

http://dev.oupeng.com/articles/7-tips-for-a-nodejs-padawan

感谢 Di Wu 同学，参考了他的译文《7 Tips for a Node.js Padawan》，某些不妥的地方做了些许修改。
或许我刚入门时就想了解这些知识。

Node.js 开发想当有趣，想当令人满足。它有3万5千多个模块供君选择，总体而言，用 node 开发一个实际应用非常简单，并且扩展起来伸缩自如。

可是对于刚接触 Node.js 开发的同学，免不了遇到一些挫折。在这篇简短的文章中，我提到了一些学习 Node.js 时遇到的问题。

技巧 1 ：开发环境使用 nodemon，生产环境使用 pm2

当你着手 Node.js  开发时，最突出的问题之一是，必须一次又一次的运行 node [file].js 。我开始学 node 时，这点相当令我抓狂。尤其是每次修改一点东西，都要 control + C 。

幸运的是我发现了一个非常棒的工具 Nodemon 。运行以下命令安装它：


npm install -g nodemon

Nodemon 了不起呀，一旦全局安装了它，你可以通过 nodemon [file].js 运行 node.js 脚本。nodemon 会监控你的代码，以及所有依赖代码的变化。如此开发 Node.js ，开发效率瞬间提高。

生产环境呢？除非你用了 Heroku，Nodejitsu 或者其它不错的 Node.js 托管服务，否则你可以试试 EC2 或者其它的云服务，通过它们运行你的 Node.js 应用。如何保证 Node.js 应用一直运行呢？

PM2 像 nodemon ，用于在生产环境运行 node 应用。它会监控你的 app 变化并重新部署它们，不同之处在于，如果 PM2 遇到事故，它将立刻重启你的 node.js 应用。

当你的应用需要多核处理的时候，凸显出了 PM2 的优势。PM2 内部集成的“负载均衡”让你轻松指定 Node 应用运行几个实例。


pm2 start app.js -i max

-i 参数用来指定运行多少个实例，此例中 PM2 使用了一个常量 max 自动扩展你的 app 运转到最大核数，不要忘记 Node 平时只运行在单核上！

技巧 2 ：Async 或者 Q

越早写 node.js 应用，越会尽早领略回调地狱之痛。如果你没见过，这有一个例子：


function register(name, password, cb){
  checkIfNameExists(name, function(err, result){
   if(err){
    return cb(“error”);
   }
   checkIfPasswordGood(password, function(err, result){
    if(err){
     return cb(“error”);
    }

    createAccount(name,password, function(err,result){
     if(err){
      return cb(“error”);
     }
     createBlog(name, function(err, result){
      sendEmail(name, function(err, result){
       callback(result);
      });
     });
    });
   });
  });
}

不怎么美的神奇代码的确存在，但你如何避免呢？

一种简单的方式是用 events ，我个人不建议这么做，它违背了一个函数的观点，可以用 events 调用只有一个用途的私有方法。

所以怎么做呢？有两个相互竞争的库：async.js 和 Q ，它们均可以避免回调地狱出现。

Async.js 或者 “async”可以轻松执行连续或者平行的函数，不需要一层一层的嵌套。

下面是 Async 支持的模式，readme 有记录。想了解 async 支持的所有模式，看看它们的代码仓库。


async.map([‘file1',’file2',’file3'], fs.stat, function(err, results){
  // results is now an array of stats for each file
});

async.filter([‘file1',’file2',’file3'], fs.exists, function(results){
// results now equals an array of the existing files
});

async.parallel([
  function(){ … },
  function(){ … }
  ], callback);

async.series([
  function(){ … },
  function(){ … }
  ]);

async.waterfall([
  function(callback){
   callback(null, ‘one’, ‘two’);
  },
  function(arg1, arg2, callback){
   callback(null, ‘three’);
  },
  function(arg1, callback){
// arg1 now equals ‘three’
callback(null, ‘done’);
}
], function (err, result) {
// result now equals ‘done’ 
});

如果我们用 async 的 waterfall 来修改之前的例子，代码将更加易读，再也不会涉及死亡金字塔了。

另一个很好的库是 Q ，这个库用到了 promises 的概念。Promise 是一个含有‘promise’方法的返回对象，他提供了一个最终返回值，非常优雅的将 javascripts 的异步特性和 node.js 紧密联系起来。

从 Q 的代码仓库页拿来的例子。


promiseMeSomething()
.then(function (value) {
}, function (reason) {
});

promise me 函数立刻返回一个对象，调用 then 将返回传入 value 值的函数。它也带一个回调函数，处理未能返回值的情况。

用非常整洁的方式避免了回调地狱。如果重写原来的例子，当 then 执行的时候才调用每个函数。


Q.fcall(checkIfNameExists)
.then(checkIfPasswordIsGood)
.then(createAccount)
.then(createBlog)
.then(function (result) {
// Do something with the result
})
.catch(function (error) {
// Handle any error from all above steps
})
.done();

如前面所说，我不喜欢创建单一目标函数。取而代之，用 “then”传递函数，仅传递一个匿名函数，当然选择器在你手中。

总之，当你掉入回调地狱时，是该关注下 async.js 或者 Q 了。

“我个人喜好？一直是 Q！”
技巧 3 ：轻松调试 Node.js 应用

如果你从一个 IDE 重度集成的语言比如 java 或者 C# 转来调试 Node.js，你一定会感到很困扰。大多数新的 node 开发者采用 “flow”调试模式，你最好的帮手是 console.log 。

但是肯定有更便利的调试方式，Node.js 内置了一个调试器你可以叫它 node debug，不过我更喜欢的 node-inspector 。

它们的 github 仓库提到“Node Inspector 是使用了 Blink 开发工具的 node.js 调试工具界面（先前的 WebKit Web Inspector）。”

简而言之，无论你选择哪个编辑器和 chrome web tools ，node-inspector 都可以调试你的应用。多爽啊。

Node Inspector 可以做一些真正酷的东西，比如实时代码修改，单步调试，作用域注入和一堆很酷的功能。

它涉及到一些设置，可以按照这里 https://github.com/node-inspector/node-inspector  的说明做。

技巧 4 ：Nodefly

一旦你的应用正常运行，你可能会问自己，如何监控它的性能，如何通过分析确保它以最佳速度运行呢。最简单的回答是使用非常棒的 Nodefly 服务。

通过一行简单的代码，Nodefly 开始监控应用程序的内存泄露， 测量 redis 用了多久，mongo 查询和一堆其他很酷的东西。

http://www.nodefly.com/

技巧 5 ：用 NPM 管理模块

node 最寻常的事情之一是通过 NPM 安装程序包。Node 有个神奇的包管理器，它会安装 package.json 清单文件里指定的模块。可是初学者都会遇到一件事情，package.json 文件中所有使用的模块如何保持最新。

总是打开 package.json 更新已安装模块的依赖属性似乎很痛苦，很多人不知道 npm 可以帮你做这些！

简单运行 npm install — save module_name ，npm 将自动更新带有正确模块和版本号的 package.json 


npm install —save module_name 

技巧 6 ：不要提交 node_modules 文件夹

我们一直谈论模块和 npm ，还有人不知道不该提交 node_modules 文件夹，最大的原因是没有必要提交。当别人检出你的代码，他们可以用 npm 安装所需模块。

你也许会说提交 node_modules 文件夹无伤大雅，但是如果检出你代码的人用跟你不同的操作系统呢，通过 npm 安装的模块是编译过的？你的应用会崩溃，检出你代码的人完全不知道为什么！

举例来说，当你安装过 bcrypt 和 sentimental 模块后，它们在主机上是编译过的，因为它们的原始组件是用 C 写的。

最优的作法是把 node_modules 文件夹加到 .gitignore 里，避免提交它。


// .gitignore node_modules/*

技巧 7 ：不要忘记 return

刚入门的 node 开发者通常容易犯个错误，callback 回调函数后面忘加 return 。虽然有时候没什么影响，多数情况你会遇到奇怪的问题，callback 回调执行了两次。

看个简单的例子：


function do(err,result, callback){
if(err){
callback(“error”);
}
callback(“good”);
}

乍一看，没什么问题。如果有错误，把 “error”传入 callback，没错则传递“good”。但是调用 callback 以后并没有停止执行，会接着调用 callback("good") 。

在复杂的代码中，加上 return 会节约数小时的调试时间。

Node.js 是个很赞的开发平台，如果你谨记这 7 条技巧，开发，调试和部署到生产环境时，可以节省不少时间，防止头发尽早变灰白。

需要 Node.js 咨询？联系 Dynamatik http://www.dynamatik.com/ 吧。