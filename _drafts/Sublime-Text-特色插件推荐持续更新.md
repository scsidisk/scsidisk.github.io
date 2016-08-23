Sublime Text 特色插件推荐, 持续更新！
==================================

https://segmentfault.com/a/1190000004949578

从 textmate 迁移到sublime text, 现在一直用 Sublime Text 3.

以下所有我了解的插件, 均可以在 package control 里面直接输入查找并安装.

#### 功能类 (内容无关)

- ColorSchemeSelector : 可以快速的切换sublime的ColorSheme, 必备插件.
- *SideBarEnhancements : 很实用的右键菜单增强插件
- *AdvancedNewFile : 方便创建文件.  cmd+opt+n
- *ProjectManager : 项目管理, 自由切换项目
- *ConvertToUTF8: 中文支持 OSX或Linux还需要插件Codecs33
- *Codecs33: 支持 ConvertToUTF8
- *SublimeREPL:  REPL的意思是Read—Eval—Print Loop, 通俗的说就是解释器, 极大的方便了调试.  装了之后支持在sublime里面内部开一个窗口ipython, 于是我们就更加不需要终端啦. 这个也有很多的插件, 我用这个实现了Java和C的REPL, 可惜这个插件的作者现在很少更新, 至今我的推送还在pull request里面.
- *FindKeyConflicts : 插件太多之后, 容易快捷键冲突, 这个软件找到冲突的快捷键, 以便于解决
- *Theme-Soda : 主题和配色方案

#### 工具类（语言无关）:

- *Alignment : 自动对齐 ctrl+cmd+a
- *apiDoc Autocompletion : apidoc 自动补齐
- *AutoFileName : 文件路径补全
- *BracketHighlighter: 光标移动到大括号左右的时候, 在屏幕左边会出现该括号的范围, 检查是不是漏了括号.
- *DocBlockr : 文档注释
- *Fold Comments : 折叠注释 cmd+shift+C
- *EditorConfig : 同一项目代码在不同编辑器下能够保持统一的代码风格
- *Git : 版本控制
- GitSavvy : 完爆上面的Git插件, 支持很多git的高级功能.
- *GitGutter: 配合Git使用, 每次编辑文档的时候都可以看到自己做了那些改动.
- *Compare Side-By-Side : 代码比较
- Origami: 神器！可以任意的操纵sublime的屏幕, 比如左右分屏, 上下分屏, 先上下再左右, 先左右再上下.
- Sublimerge Pro:  diff神器. 比较两边文档的不同, 同步下拉. 付费软件
- *Terminal : 可以在当前project的位置打开terminal
- MacTerminal: 快捷键可以在当前project的位置打开terminal, 支持iTerm 2 哦！
- *Terminality: 有了terminality, python, C和shell都可以在sublime Text里面支持终端输入啦.
- QuickMail: 一个可以在sublime里面发送和接受邮件的插件, 写了一段代码, 直接发送, 很方便.
- *SublimeLinter : 代码检测的工具, 支持： C/C++, Java, Python, PHP, JS, HTML, CSS, etc
- *CnDict:  中英文字典软件, 快捷键查词, 目前支持金山词霸和有道词典.
- Hex Viewer: 非常好的二进制查看和编辑器.
- *SublimeCodeIntel : 按住ctrl+鼠标左键, 可以实现自定义函数之间的跳转, 方便查找和修改函数内容和读写代码！
- *Ctags: 通过build index, 可以实现比内置的GO TO DEFINITION功能更好的“跳转到定义”的功能.
- WhocalledFunctionFinder: 模拟的是ctags的逆向操作, 从定义跳转到调用. 和ctags配合, 基本上和IDE差不多了, 只是正则匹配, 不能够真正理解你的代码.
    备注: 对于python而言, ctags 和whocalled都是不必要的, 一个anaconda就够了.
- FuzzyFileNav : 可以把sublime text当作一个简易的文件管理器使用.
- HiveOpener: 可以自己设定快捷列表, 用快捷键打开文件或者文件夹.
- YouCompleteMe: 语义分析自动补全啦！安装复杂, 但是你绝对不会为之后悔的！Ctags, WhocalledFunction, Anaconda可以关闭啦, 一切都交给YouCompleteMe！直接支持C family的语言, Python, Rust,  Go, 通过插件可以支持Javascript,  Java,  基本上主流的语言都可以用了. 我会写一篇独立的文章来讲YouCompleteMe和sublime text的安装和配合的.
- YCMDCompletion: 这个就是配合YouCompleteMe后端的. 不过建议只用来进行C系语言的语义分析, 因为python有anaconda, Rust有RustAutoComplete.
- *TrailingSpaces : 高亮行末空格保存的时候删除
- *Evaluate : 计算页面中的算式 cmd+shift+e

#### 配置文件类

- *nginx : nginx配置文件语法支持

#### PHP 类

- PhalconPHP Completions
- *phpfmt
- Phpcs
- *SublimeLinter-php : 代码检测的工具

### Golang类

- *GoSublime

#### Python 类

- MagicPython:  更好的python语法高亮, 类似的还有Python improved, 选一个就好了.
- Anaconda:  python必备, 转变成轻量级IDE, 实时纠错, check style, 自动完成. 我之前使用过SublimeCodeIntel 和Jedi, 但是后来还是把这俩卸载掉了改用Anaconda. Ananconda的python格式检查和自动纠错, 已经全面超越了sublime linter的pep8和pyflakes.
- SublimeLinter-pep8,  SublimeLinter-pyflakes: 我唯独没有开启的功能就是Ananconda的linter. 因为在语法查错, 规范格式方面, 我还没有找到比sublimeLinter的插件pep8和 pyflake 更好的. 开启着sublimeLinter写python, 妈妈再也不用担心我写的代码不合规范了.
- PyYapf: 有的时候, pep8和pyflake也无能为力, 可以用PyYapf格式代码.

#### Java 类:

- Javatar :这是一个类似于Ananconda在python里的存在. 要把sublime变成轻量Java IDE. 部分实现了, 还比较初级. 但是Javatar是我们目前所有的java插件中最好的, 也是唯一的选择.
- SublimeAStyleFormater:  Java的自动格式整理, 类似于上面的PyYapf.

#### 前端类

- *SublimeLinter-jshint : 代码检测的工具
- *JSHint: 检查Javascript的错误.
- *JsFormat
- CSS3 :  一个更好的CSS语法高亮.
- HTML5: 一个更好的HTML5语法高亮.
- *Babel:支持ES6,  React.js, jsx代码高亮, 对 JavaScript, jQuery 也有很好的扩展
- *Pretty JSON
- *JavaScript Completions
- *JavaScript Snippets
- *JavaScriptNext – ES6:  一个更好的JavaScript 语法高亮.
- *jQuery
- *Sass
- *SassBeautify
- *HTMLBeautify
- *ColorPicker
- TypeScript : 微软的js,  具有类型的JavaScript的超集并可以编译成普通的JavaScript代码.
- LiveReload: 压轴神器！ 这个插件非常重要. 在浏览器 （chrome, firefox, safari上也装上相应的插件）, 在sublime里面的修改, 在浏览器里面可以实时的看到. 有了LiveReload, 极大提高了我编码的效率, 之前简直痛苦, 微调网页元素能够实时预览的意义怎么强调都不为过啊. 可惜不再更新了……
- Web Inspector: 这才是真正的压轴神器, 什么LiveReload那都弱爆了, 一个不更新的东东, 继续支持是没有前途滴！ web inspector在各个方面都比live reload做得更好, 还能够单点调试！

#### Rust类

- RustAutoComplete: Rust 的语义分析自动完成, 基于racer.
- Rust: Rust 的syntax file, 现在已经整合进sublime 安装包了.

#### 文档/写作类:

- *OmniMarkupPreviewer
- *Markdown Preview
- MarkdownEditing: 高亮显示语法,编程语言的语法高亮显示, 配合Pandoc可以任意转成Word, PDF或者RTF.
- Latexing: Latex写作是学者的基本, Latexing插件是目前最好的Latex插件没有之一, 虽然是付费的, 但是价格不贵, 并且支持、更新都非常到位. Latexing也是支持实时预览的. 在OSX下面配合Skim PDF浏览器, 可以随时编译latex源代码并且定位到PDF上, 非常方便写作. 有了Latexing之后, 我彻底抛弃了Latexian, latex Pad等一干软件, 用sublime Text作为自己工作的主要编辑器.
- Pandoc: 用法我在上面已经讲完了……
- WordCount:  名字说明一切, 就是在状态栏里面显示字数统计的小插件.
- *AsciiDoc: 文章书写格式
- Evernote : 变身 Evernote 大咖

#### Linux 管理:

- Generic Config:  Linux Config 文档的语法高亮.
- SFTP


