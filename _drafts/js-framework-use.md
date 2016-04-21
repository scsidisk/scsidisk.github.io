

## jquery docment ready

允许使用以下三种语法：
语法 1
$(document).ready(function)
语法 2
$().ready(function)
语法 3
$(function)


These are the different types of Document Ready functions typically used in jQuery (aka jQuery DOM Ready). A lot of developers seem to use them without really knowing why. So I will try to explain why you might choose one version over another. Think of the document ready function as a self-executing function which fires after the page elements have loaded.

See Where to Declare Your jQuery Functions for more information on how to use the Document Ready Functions.

Document Ready Example 1
$(document).ready(function() {
    //do jQuery stuff when DOM is ready
});

Document Ready Example 2
$(function(){
    //jQuery code here
});
This is equivalent to example 1… they literally mean the same thing.

Document Ready Example 3
jQuery(document).ready(function($) {
    //do jQuery stuff when DOM is ready
});
Adding the jQuery can help prevent conflicts with other JS frameworks.

Why do conflicts happen?
Conflicts typically happen because many JavaScript Libraries/Frameworks use the same shortcut
name which is the dollar symbol $. Then if they have the same named functions the browser gets
confused!

How do we prevent conflicts?
Well, to prevent conflicts i recommend aliasing the jQuery namespace (ie by using example 3 above).
Then when you call $.noConflict() to avoid namespace difficulties (as the $ shortcut is no longer available)
we are forcing it to wrtie jQuery each time it is required.

jQuery.noConflict(); // Reverts '$' variable back to other JS libraries
jQuery(document).ready( function(){
         //do jQuery stuff when DOM is ready with no conflicts
   });

//or the self executing function way
 jQuery.noConflict();
 (function($) {
    // code using $ as alias to jQuery
})(jQuery);
Document Ready Example 4
(function($) {
    // code using $ as alias to jQuery
  $(function() {
    // more code using $ as alias to jQuery
  });
})(jQuery);
// other code using $ as an alias to the other library
This way you can embed a function inside a function that both use the $ as a jQuery alias.

Document Ready Example 5
$(window).load(function(){
        //initialize after images are loaded
   });
Sometimes you want to manipulate pictures and with $(document).ready() you won’t be able to do that
if the visitor doesn’t have the image already loaded. In which case you need to initialize the
jQuery alignment function when the image finishes loading.

You could also use plain JavaScript and append a function call the the body tag in the html, use this only if your not using a JS framework.

## ES6

- jscs <http://jscs.info/>
- eslint
- http://es6.ruanyifeng.com/#README
- ES5 <http://pij.robinqu.me/>
- Babel 一个广泛使用的转码器，可以将ES6代码转为ES5代码
- http://babeljs.io/repl/


## webpack


## browserify

## commonjs


## AMD


## NPM

- run

## Bower


## Galup


## Grant



===============================================

2016年的前端技术栈展望
 Web前端  dwqs  2016-03-18 00:18  758 人阅读  0评论
如果你正在规划一个新的前端项目或者重构现有的项目，可能就会发现，你已经跟不上前端生态的变化了，因为现在有太多的技术栈供你选择：React、Flux、Angular、Aurelia、Mocha、Jasmine、Babel、TypeScript,、Flow……这些技术栈的出现试图开发变得简单，但有一部分却增加了学习成本和项目维护的不稳定性，因为技术栈的变化速度太快了。

commic

尽管这样，但是好消息是，前端生态的技术栈开始变得稳定了，部分项目进行了合并，技术的最佳实践也越来越清晰了，有人已经开始基于上述的技术栈而不是新框架来规划项目了。

本文主要介绍一些我在 Web 应用中所涉及和推崇的技术，由于一些技术还存在争议，所以基于个人所见和经验，对每一项技术只做简单的介绍和分析。

核心库：React

React

组件化的 UI 方便开发、维护以及管理，学习成本也低
JSX 的语法能在 HTML 中极大发挥 js 的优势
天生适合结合 Flux 和 Redux
强大而活跃的社区，各种周边层出不穷
单向数据流比双向数据绑定的方式更适合复杂应用程序
支持服务端渲染
应用生命周期：Redux

redux

React 提供了视图和组件层，但还需要某个东西来管理应用的状态和生命周期。Redux 则成为首选。

和 React 一起，Facebook 提供了一种被称为 Flux 的单向数据流设计模式。Flux 基本实现了简化组件状态管理的承诺，但也带来了新的问题，如怎么存储状态、发送 Ajax 请求等。

为了解决上述两个问题，社区产生了大量基于 Flux 模式的框架：Fluxible, Reflux, Alt, Flummox, Lux, Nuclear, Fluxxor 等等。

最终，一个类 flux 的实现成功吸引了社区的注意，它就是 Redux。在 Redux 中，只有一个集中的 store（这个 store 可以由多个组件的子 store 组成） 和资源中心，组件状态的所有变化都由 纯函数 作控制，Reducer 则负责维护组成 store 的数据。相比 Flux 来说，Redux 的数据流向和控制更加清晰。

语言：ES6 和 Babel

babel

可以抛弃 CoffeeScript，因为 ES6 拥有了它的大多数优良特性，而作为标准，ES6 已经被大多数新版本的浏览器支持。Babel 则能将 ES6 转为 ES5 ，让 Web 应用能得到更多稍低版本浏览器的支持。TypeScript 和 Flow 都为 JavaScript 提供了静态类型系统，使用静态类型检查，可以有效捕获错误，减少测试工作量。

格式：ESLint

eslint

ESLint 不仅是一个代码检查工具，还完美支持 ES6，并提供了 React 插件。JSLint 已经过时，ESLint 是可以取代 JSHint 和 JSCS 的组合的工具。

你可以按照你的代码风格来配置 ESLint，而我则强烈推荐 Airbnb 的 JavaScript 开发规范，而这规范中的大部分能通过 ESLint airbnb config 强制执行。

依赖管理：NPM，CommonJS 和 ES6 模块

npm

现在应该用 npm 来接管 web 应用的一切！类似 Browserify 和 Webpack 的构建工具为 NPM 在 Web 领域注入了强大的动力。NPM 使应用的版本管理变得非常容易，也将更多地与 Node.js 生态系统接触，但目前 NPM 对于 CSS 的处理尚不足够完善。

使用 npm时，可能需要考虑怎么在应用要部署的服务器上处理构建问题。与 Ruby 的 Bundler 不同，对于依赖的版本，npm 是通过通配符来管理，并且依赖包可以在你完成编码和开始部署之间的任何时刻被改变，但能通过 shrinkwrap 来冻结依赖(建议使用 Uber 的 shrinkwrap 来获取更好的一致性输出)，也可以使用类似 Sinopia 的工具来托管自己的私有 npm 服务器。

构建工具：Webpack

webpack

除非你喜欢在页面添加数百个脚本标签，否则，你需要使用构建工具来打包页面资源。此外，还需要某些工具让浏览器支持 NPM 包。这就是 Webpack 应该干的事：

组件化的管理
支持主流的模块加载方式(AMD，CommonJS，globals)
内置模块损坏修复机制
很好的 CSS 处理方式和支持热加载
几乎能加载所有 东西，并支持按需加载
高效的性能优化方案
相对于 Gulp or Grunt，Webpack 能更好地处理各种资源，但 Gulp or Grunt 在执行其它类型的构建任务时还是很有用的。对于基本任务(如 运行 webpack 或者 eslint)，推荐使用 NPM Script

测试：Mocha+Chai+Sinon

测试

对于 JavaScript 的单元测试，有很多框架可以选择，如 Jasmine，Mocha，Tape，AVA 和 Jest，我个人对于测试框架有如下要求：

可以在浏览器运行，易于调试
执行速度要够快
易于处理异步测试
易于在命令行中使用
可以兼容任意断言和数据模拟的第三方库
第一条标准就排除了 Ava 和 Jest。

我个人喜欢断言库 Chai(插件丰富)和 Mocha(对异步支持友好) 。此外，推荐使用 Chai as promise 和 Dirty Chai 来避免一些不必要的问题。Webpack 的 mocha-leader 插件允许开发者自动执行测试。

对于 React 而言，开发者可以参考一下 Airbnb 的 Enzyme 和 Teaspoon。

工具库：Lodash

JavaScript 不像 Java 或者 .NET 有自身的核心工具库，因而开发者都需要从外面引入工具库。Lodash 就是一个功能非常齐全的工具库，并且由于惰性执行 等特性，其性能很优越。此外，Lodash 支持开发按需引用资源。在 4.x 版本中，Lodash 为偏爱函数式编程的开发者提供了一个“函数式开发”模式。

Http请求：Fetch

许多基于 React 的应用已经不需要 jQuery 了，除非是接收一个旧有项目或者第三库依赖 jQuery，否则就找不到使用 jQuery 的理由了，也以为能取代 $.ajax 了。

Fetch 基于 promise，使用 fetch 能使项目保持简单，但目前只有 Firefox 和 Chrome 支持，对于其他浏览器，需要引入 polyfill，个人推荐使用 isomorphic-fetch 来确保能覆盖各浏览器端，也包括服务端。

关于更多为什么 promise 很重要的细节，可以戳: asynchronous programming
样式：CSS 模块

CSS 模块化是比较滞后的领域，SASS 目前朝着这个方向发展。在 JavaScript 项目中，node-sass 是实现 CSS 模块化的一种不错的方式，它是一个会和 Node 版本保持同步更新的 C 语言库。

目前的 CSS 模块化方案缺少一些最佳实践，如引用导入(reference import)和本地 URL 重写(native URL rewriting)。LESS 是不错的 CSS 预处理器，但由于缺少 SASS 的许多功能而不受追捧。PostCSS 是很有希望实现 CSS 模块化的。CSS 模块 是一件值得考虑的事情，它会阻止 CSS 的部分“重叠”和冲突，保持 CSS 代码的干净和独立性，不用担心类名会被覆盖或者必须为类名指定“非常明确”的名称。

JavaScript 同构/通用

通用或同构的 JavaScript 是指能同时运行在客户端和服务端。出于性能考虑和 SEO 的目的，JavaScript 同构最初用于服务端的预渲染。使用 React 可以实现同构 JavaScript，但是并不简单，它提高了程序的复杂度，限制了开发者可选的工具和第三方库。

如果你正在构建一个 B2C(Business to Customer) 网站，如电商网站，可以考虑 JavaScript 同构，因为除了去到指定路由别无选择，对于内部网站或 B2B(Business to Business) 应用，性能就显得不重要了。

API

最近，总有人问要怎么去设计 API，每个人都钻进了 RESTFul API 的“万花筒“，认为 SOAP 过时了。在业界有很多关于 API 设计的协议和规范，如 HATEOAS，JSON API，HAL，GraphQL 等。

GraphQL 赋予了客户端进行任意查询的能力。搭配 Relay，可以更好地处理客户端的状态和缓存。不过，创建 GraphQL 的服务端接口的难度还较大，且大多数的文档都是面向 Node.js 的。

尽管业界有诸多协议，但我并不认为有一个完美的解决方案，对 API 设计，我有自己的认知：

可预测，遵循一致性协议
支持在一次查询中获取多个实体
支持更新
便于调试和使用
如果你正在使用 RESTful，可以参考 Swagger 文档来编写 API。

桌面应用：Electron

Electron 是 Atom 编辑器的基础，能够使用前端技术来构建桌面应用，其核心就是能在 Chrome 窗口中渲染 GUI 的 Node.js。Electron 可以操作系统本地 API，并且不受浏览器的沙盒安全限制。跟其它桌面应用一样，Electron 能帮开发者完成应用的打包、发布、安装和自动更新，这是创建跨平台软件最简单的方式，并且 Electron 有完整的文档和活跃的开发社区。

尽管 nw.js 已经存在多年，但是 Electron 比它更稳定和易于使用。

文章最后，提供一个基于 Electron、React 和热加载的 demo：Boilerplate。

本文参照 @Francois Ward 的文章翻译，有增删(不服？？！！来xx我丫~~O(∩_∩)O哈哈~)
转载请注明：淡忘~浅思 » 2016年的前端技术栈展望


================================
其实确认了选择标准的话，都不是很难选择，我选择的标准是：“教起来尽量的简单”，而且之前选择的莫名其妙的用了三年左右。

gulp 基本可以满足所有功能，自己加新的简单方便，grunt编写个新的plugin 难度大了点复用性还不是很好，后面一个听过看过一些教程然后放弃了。不过这个是先选择了grunt之后再换到gulp，因为当年gulp还比较新。

requirejs api 稳定用了两年都没有大变过，开箱即用。browserify webpack 要么加载前要编译，要么要起server都不适合快速配置环境，放弃。

npm bower component 只用npm。 bower还差了点，而且感觉上怪怪的，总觉得和npm一样从来不过滤垃圾。component 流行度不够。

commonjs amd es6 后端cjs ，前端amd， 很好区分。es6流行度不够而且选择了前两者就和es6没太大关系了，尽管有项目也在用es6语法的typescript。

框架首选backbone也是因为API四五年没太大变化，扩展起来也容易。google组的不跟，觉得他们的复杂而且口碑差，迁移部分概念到backbone的framework即可。ember的上手难度远超backbone。正在研究react和backbone的整合可能性。

模板系统选择doT至少一两年没啥更新了，因为可以编译成无依赖的纯js配合requirejs optimizer。handlebar那玩意编译成js还需要加载一个min做支持，虽然提升了复用性，但是optimizer的时候需要考虑的比无依赖的多。



===========================================
Node.js：现代工业化前端的基础；
RequireJS：AMD规范，即将过时的 JavaScript 模块化方案；
Bower：前端模块源；
npm；前端工具源，另一个潜在的前端模块源；
Browserify：即将过时的基于 CommonJS 的前端模块化方案；
Less：等 CSS 增强工具；
Gulp：前端构建工具，如果你在前端开发中不需要使用类似工具的话，我只能呵呵；