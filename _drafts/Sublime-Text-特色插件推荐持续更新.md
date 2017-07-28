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
- *SublimeREPL:  REPL是语言解释器, 允许你运行各种语言（NodeJS ， Python，Ruby 等等）
- *FindKeyConflicts : 插件太多之后, 容易快捷键冲突, 这个软件找到冲突的快捷键, 以便于解决
- *Theme-Soda : 主题和配色方案
- WakaTime – 记录你在不同项目上面的时间

#### 工具类（语言无关）:

- *Alignment : 自动对齐 ctrl+cmd+a
- *apiDoc Autocompletion : apidoc 自动补齐
- *AutoFileName : 文件路径补全
- *BracketHighlighter: 光标移动到大括号左右的时候, 在屏幕左边会出现该括号的范围, 检查是不是漏了括号.
- *DocBlockr : 代码块注释，支持多种语言，/*:回车创建一个代码块注释 /**:回车在自动查找函数中的形参等等。
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
- SublimeTmpl 快速生成文件模板, 支持html、css、javascript、php、python、ruby六种类型
- Bracket Highlighter 用于匹配括号，引号和html标签。对于很长的代码很有用。安装好之后，不需要设置插件会自动生效
- ClickableURLs：可点击的URL
- CSS Compact Expand: CSS属性展开收缩
- SublimeLinter: 高亮提示用户不规范和错误的写法，支持 js、CSS、HTML、Java、PHP 等十多种开发语言。

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
- *JsFormat 格式化js代码，懂者自懂；强迫症Coder必备！默认快捷键Ctrl+Alt+F。
- CSS3 :  一个更好的CSS语法高亮.
- HTML5: 一个更好的HTML5语法高亮.
- *Autoprefixer: 这是一款CSS3私有前缀自动补全插件；
- *Vue Syntax Highlight: Vue(*.vue)高亮插件
- *Babel:支持ES6,  React.js, jsx代码高亮, 对 JavaScript, jQuery 也有很好的扩展
- *Pretty JSON
- *JavaScript Completions
- *JavaScript Snippets
- *JavaScriptNext – ES6:  一个更好的JavaScript 语法高亮.
- *jQuery
- *Sass
- *SassBeautify
- *HTMLBeautify
- *ColorPicker: 颜色选择器。安装完成后，只要按下Ctrl / Cmd + Shift + C 快捷键。
- Javascript-API-Completions 支持Javascript、JQuery、Twitter Bootstrap框架、HTML5标签属性提示的插件。
- TypeScript : 微软的js,  具有类型的JavaScript的超集并可以编译成普通的JavaScript代码.
- LiveReload: 压轴神器！ 这个插件非常重要. 在浏览器 （chrome, firefox, safari上也装上相应的插件）, 在sublime里面的修改, 在浏览器里面可以实时的看到. 有了LiveReload, 极大提高了我编码的效率, 之前简直痛苦, 微调网页元素能够实时预览的意义怎么强调都不为过啊. 可惜不再更新了……
- Web Inspector: 这才是真正的压轴神器, 什么LiveReload那都弱爆了, 一个不更新的东东, 继续支持是没有前途滴！ web inspector在各个方面都比live reload做得更好, 还能够单点调试！
- HTML-CSS-JS Prettify: 集成格式化（美化）html、css、js三种文件类型的插件.
- CSScomb: CSS属性排序, 按Ctrl+Shift+C
- Emmet(Zen Coding): 快速生成HTML代码段的插件，还支持多种IDE
- YUI Compressor：压缩JS和CSS文件，按F7键后，在同目录下生成压缩文件，需要安装java的JDK。

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


## Sublime不可不知的实用技巧

- Ctrl+O(Command+O)可以实现头文件和源文件之间的快速切换
- 通过 View -> Side bar 可在左侧显示当前打开的文件列表;默认快捷键 Ctrl+k+b
- ST3虽然不像notepad++可以在sidebar上显示函数列表，但是可通过Ctrl+R查看
- 通过 Preference -> Key binding user 可根据个人操作习惯自定义快捷键（包括ST3自带的和插件的）
- 双击可选中光标所在单词，三击可选中光标所在行(等同于Ctrl＋L(Command+L));
- Ctrl+Shift+T可以打开之前关闭的tab页，这点同chrome是一样的
- Ctrl+R定位函数；Ctrl+G定位到行；
- 单个文件批量修改：纯相同的内容：选中需要修改的内容Alt+F3(Mac下默认的是Ctrl+Command+G) ， 或者连续 Ctrl+D(Win) /连续 Command+D（Mac）之后重新写即可，使用Ctrl + U进行回退，使用Esc退出多重编辑。
- 有时我们需要对一片区域的所有行进行同时编辑，Ctrl+Shift+L可以将当前选中区域打散，然后进行同时编辑：
- 有打散自然就有合并，Ctrl + J(mac下Command＋J)可以把当前选中区域合并为一行：
- 在Ctrl + P(Command+P)匹配到文件后，我们可以进行后续输入以跳转到更精确的位置：
    - @ 符号跳转：输入@symbol跳转到symbol符号所在的位置
    - # 关键字跳转：输入#keyword跳转到keyword所在的位置
    - : 行号跳转：输入:12跳转到文件的第12行。
- Ctrl + Enter(Mac~Command+Enter)在当前行下面新增一行然后跳至该行；Ctrl + Shift + Enter在当前行上面增加一行并跳至该行。
- Sublime Text的查找有不同的模式：Alt + C切换大小写敏感（Case-sensitive）模式，Alt + W切换整字匹配（Whole matching）模式，除此之外Sublime Text还支持在选中范围内搜索（Search in selection），这个功能没有对应的快捷键，但可以通过以下配置项自动开启。
        “auto_find_in_selection”: true
这样之后在选中文本的状态下范围内搜索就会自动开启，配合这个功能，局部重命名（Local Renaming）变的非常方便：
- Mac下的Command+←/→是从一端移动到另一端；相应的，Command + Shift + ←/→是从一端选择到另一端。Mac 下 option 加上左右键可以逐词移动；
- Mac下的Command + ↑/↓是从当前行移动到头/尾；相应的，Command + Shift + ↑/↓是从当前行选择到头/尾；
- 使用Ctrl + N在当前窗口创建一个新标签，Ctrl + W关闭当前标签，Ctrl + Shift + T恢复刚刚关闭的标签。
- 编辑代码时我们经常会开多个窗口，所以分屏很重要。Windows下：Alt + Shift + 2进行左右分屏，Alt + Shift + 8进行上下分屏，Alt + Shift + 5进行上下左右分屏（即分为四屏）。
- Sublime Text基本的手动格式化操作包括：Ctrl + [向左缩进(等同于将一块选中Shift+Tab)，Ctrl + ]向右缩进(等同于将一块选中后Tab键)，注解： Ctr+[ 和 Ctr+[ 针对一块连续内容使用，无需选中；此外Ctrl + Shift + V可以以当前缩进粘贴代码（非常实用）。
- 安装下Clipboard-history插件～（粘贴板历史记录）才行啊)（Mac下Command＋Shift＋V），粘贴之时可以调出之前粘贴过的内容（以一个轻量弹框显示以供选择），哇哦，才发现这个功能，感觉棒棒哒😄😄。
- Sublime text 删除插件步骤：“Ctrl+Shift+P”—“Remove Package”—“找到需要删除的插件，并点击即可删除”;
- 作为强大而小巧，性感且快捷的SublimeText，怎么能够允许不时弹个框提醒你购买或者别的，并且顶部有未注册这样破坏美感的存在呢？OK，输入Sublime text 3最新版破解方法中提供的注册码，就妥妥的哦了。


<!--

推荐！Sublime Text 最佳插件列表

朋友们你们好！我尝试着收集了最佳的ST插件，这些插件真的会改善你的工作流程。我搜索了很多网站，下面是我的成果。

WebInspector

在 JavaScript调试方面，这是一个令人惊讶的工具，Sublime上的完整的代码检查工具。
功能：使用绝对路径储存在用户设置中的项目断点，控制台，分步和断点调试器，栈追踪。这些都能够很棒的工作！而且Mozilla还提供了一个插件Fireplay让你连接到Firefox 开发工具和最简单的调试器JSHint

视频

Emmet

编辑器中最流行的插件之一。Emmet，前身Zen Coding也是web开发者提高生产力最有效的方法之一。按下Tab键，Emmet就能把一个缩写展开成一个HTML和CSS代码块，我想提一下Hayaku-集合了方便的层叠样式表缩写。

包含最棒的技巧的视频，来自项目作者

Git

这个插件的实质，看一下它的名字就知道了–它提供了使用我们的最爱的编辑器直接和Git协同工作的机会。使用这种方式与Git协同工作会节省您大量的时间。首先：您不需要时常的在Sublime和终端间相互切换。另外：它具有tag自动补全功能，写add就足够了，而不是git add -A。第三点：它具有快速提交功能(quick)，一个命令添加所有变化并全部提交。

如果你只是想利用Git来获取远程仓库的内容，我推荐使用Nettuts+ Fetch.

有个叫Glue的插件，会在界面下方显示一个小窗口，你可以在那里写Shell脚本。这样一来，你的编辑器就不仅仅局限于使用Git了。

GitGutter & Modific

这些插件可以高亮相对于上次提交有所变动的行，换句话说是实时的diff工具。
1

BracketHighlighter

好极了！打开和折叠代码的某一部分就应该是这个样子的。
2

EditorConfig

3

EditorConfig帮助开发者在不同的编辑器，IDE之间定义和维护统一的编程风格。EditorConfig工程包含一个文件，定义了编程风格，文本编辑器插件集合，让编辑器可以读取该文件并依照它来定义风格。例如.editorconfig文件：


; html-script: false ]# top-most EditorConfig file
root = true

# Unix-style newlines with a newline ending every file
[*]
end_of_line = lf
insert_final_newline = true

# 4 space indentation
[*.py]
indent_style = space
indent_size = 4

# Tab indentation (no size specified)
[*.js]
indent_style = tab

# Indentation override for all JS under lib directory
[lib/**.js]
indent_style = space
indent_size = 2

# Matches the exact files either package.json or .travis.yml
[{package.json,.travis.yml}]
indent_style = space
indent_size = 2
; html-script: false ]# top-most EditorConfig file
root = true

# Unix-style newlines with a newline ending every file
[*]
end_of_line = lf
insert_final_newline = true

# 4 space indentation
[*.py]
indent_style = space
indent_size = 4

# Tab indentation (no size specified)
[*.js]
indent_style = tab

# Indentation override for all JS under lib directory
[lib/**.js]
indent_style = space
indent_size = 2

# Matches the exact files either package.json or .travis.yml
[{package.json,.travis.yml}]
indent_style = space
indent_size = 2
Sublimall

一个简洁的插件，可以让你在不同的Sublime Text 编辑器间同步所有的配置（设置，插件，打开的文件等等）所有的一切都是免费的，你只需要创建一个账户即可。是BufferScroll的一个更简约的替代品。

4

译者注：现在暂时无法注册
>Max registration reach
I’m sorry about that, don’t forget that it’s a beta version of Sublimall.
Registrations will been soon re-opened!
Geoffrey.

AllAutocomplete

传统的Sublime Text自动补全插件仅仅在当前文件下工作。AllAutocomplete 可以搜索全部打开的标签页，这将极大的简化开发进程。当然，还有一个插件叫 CodeIntel，实现了一些IDE的功能并且为一些语言提供了“代码情报”： JavaScript, Mason, XBL, XUL, RHTML, SCSS, Python, HTML, Ruby, Python3, XML, Sass, XSLT, Django, HTML5, Perl, CSS, Twig, Less, Smarty, Node.js, Tcl, TemplateToolkit, PHP.
5

SublimeREPL

对开发者来讲这个可能是最有用的插件之一了。SublimeREPL 可以直接在编辑器中运行一个解释器，支持很多语言：
Clojure, CoffeeScript, F#, Groovy, Haskell, Lua, MozRepl, NodeJS, Python, R, Ruby, Scala, shell
6

DocBlockr

DocBlockr会成为你编写代码文档的有效工具。当输入/**并且按下Tab键的时候，这个插件会自动解析任何一个函数并且为你准备好合适的模板
12

Floobits

7
SublimeText, Vim, Emacs, IntelliJ IDEA极佳的扩展工具，它使得开发者可以在从不同的编辑器合作编写代码。

AutoFileName

自动补全文件路径-非常方便。没有废话。
8

ColorPicker

通常，如果我们需要一个调色盘的时候，我们习惯使用Photoshop或是Gimp。但是一个完整的选色工具可以直接在你的编辑器中使用- Ctrl/Cmd + Shift + C。还有两个插件 GutterColor 和 ColorHighlightergutter可以在gutter中显示很棒的色彩高亮，简化了色彩代码的定位。
13

Colorcoder

高亮所有变量，因此可以极大的简化代码定位。尤其是对那些有阅读障碍的程序员非常有帮助。
9

PlainTasks

杰出的待办事项表！所有的任务都保持在文件中，所以可以很方便的把任务和项目绑定在一起。可以创建项目，贴标签，设置日期。有竞争力的用户界面和快捷键。
10

MarkdownEditing

可能是Markdonw最好的插件了：语法高亮，缩略词，自动补全，配色方案。你也可以尝试使用MarkdownPreview作为替代解决方案。
11

最后

Sublime SFTP
CTags – 让Sublime Text支持CTags.
SideBarEnhancement – 为侧边栏添加很多额外的功能.
ActualVim – Vim in Sublime – 两个最爱的编辑器合二为一.
SublimeLinter – 行内语法检测插件，支持： C/C++, Java, Python, PHP, JS, HTML, CSS, etc.
CSScomb – CSS代码风格格式化.
FixMyJS, Jsfmt and JsFormat – JS/JSON代码风格格式化.
AStyleFormatter – C/C++/C#/Java 代码风格格式化.
SVG-Snippets – 一套 SVG 代码片段.
Inc-Dec-Value – 增加或减少数字, 日期, 十六进制彩色值等等。
Trailing Spaces – 高亮空白结尾并快速删除它们
Alignment – Package Control作者写的简单到极致的多行选择和多行选择对齐插件
Placeholders – 带有文本，图片，列表，表格等的占位用代码片段
ApplySyntax – 快速语法检测
StylToken – 允许以不同的颜色高亮特定的一段文本 (类似和notepad++ 的Style token功能).
EasyMotion – 快速跳转到任何当前激活视图而已看到区域的字符
ZenTabs 和Advanced?New?File – 改进默认tab样式和文件创建.
EncodingHelper – 猜测文本的编码方式，在状态栏显示，从不同的编码形式转换到UTF-8
Gist – 同步GitHub Gist和Sublime (ST2).
Clipboard History (ST2) – 为的剪切板保存历史记录

主题和配色方案

Soda
Spacegray
Flatland
Tomorrow
Base 16
Solarized
Predawn
itg.flat
其他所有的配色方案和主题.


        "AdvancedNewFile",
        "Alignment",
        "apiDoc Autocompletion",
        "AsciiDoc",
        "AutoFileName",
        "Autoprefixer",
        "Babel",
        "BracketHighlighter",
        "Chinese-English Bilingual Dictionary",
        "Codecs33",
        "ColorPicker",
        "Compare Side-By-Side",
        "ConvertToUTF8",
        "CTags",
        "DocBlockr",
        "EditorConfig",
        "Evaluate",
        "Evernote",
        "FindKeyConflicts",
        "Fold Comments",
        "Git",
        "GitGutter",
        "Glue",
        "GoSublime",
        "HTMLBeautify",
        "JavaScript Completions",
        "JavaScript Snippets",
        "JavaScriptNext - ES6 Syntax",
        "Javatar",
        "Jinja2",
        "jQuery",
        "JsFormat",
        "JSHint",
        "nginx",
        "OmniMarkupPreviewer",
        "Package Control",
        "phpfmt",
        "Pretty JSON",
        "ProjectManager",
        "Sass",
        "SassBeautify",
        "SideBarEnhancements",
        "SublimeCodeIntel",
        "SublimeLinter",
        "SublimeLinter-jshint",
        "SublimeLinter-php",
        "SublimeREPL",
        "Terminal",
        "Terminality",
        "Theme - Soda",
        "TrailingSpaces"

-->