---
layout: post
title: "在 Mac 上使用 Sublime Text 3"
date: 2015-08-15
author: scsidisk
categories: MacOSX
tags: Mac, Sublime
---

## 快捷键

按键符号 ⌘(command),^(control),⌥(option),⇧(shift),⌫(delete®®®)

### 打开/前往

⌘T			前往文件
⌘⌃P		前往项目
⌘R			前往 method
⌘⇧P		命令提示
⌃G			前往行
⌘KB			开关侧栏
⌃ `			python 控制台
⌘⇧N		新建窗口

### 编辑

⌘L			选择行 (重复按下将下一行加入选择)
⌘D			选择词 (重复按下时多重选择相同的词进行多重编辑)
⌃⇧M		选择括号内的内容
⌘⇧↩		在当前行前插入新行
⌘↩			在当前行后插入新行
⌃⇧K		删除行
⌘KK			从光标处删除至行尾
⌘K⌫		从光标处删除至行首
⌘⇧D		复制(多)行
⌘J			合并(多)行
⌘KU			改为大写
⌘KL			改为小写
⌘ /			注释
⌘≈ /		块注释
⌘Y			恢复或重复
⌘⇧V		粘贴并自动缩进
⌃ space		自动完成(重复按下选择下一个提示)
⌃M			跳转至对应的括号
⌘U			软撤销（可撤销光标移动）
⌘⇧U		软重做（可重做光标移动）

### XML/HTML

⌘⇧A		选择标签内的内容
⌘⌥ .		闭合当前标签

### 查找/替换

⌘F			查找
⌘⌥F		替换
⌘⌥G		查找下一个符合当前所选的内容
⌘⌃G		查找所有符合当前所选的内容进行多重编辑
⌘⇧F		在所有打开的文件中进行查找

### 拆分窗口/标签页

⌘⌥1		单列
⌘⌥2		双列
⌘⌥5		网格 (4组)
⌃[1,2,3,4]		焦点移动至相应组
⌃⇧[1,2,3,4]	将当前文件移动至相应组
⌘[1,2,3…]		选择相应标签页

### 书签

⌘F2			添加/去除书签
F2			下一个书签
⇧F2			前一个书签
⌘⇧F2		清除书签

### 标记

⌘K space	设置标记
⌘KW			从光标位置删除至标记
⌘KA			从光标位置选择至标记
⌘KG			清除标记

```
Editing
Keypress	Command
⌘ + X	Cut line
⌘ + ↩	Insert line after
⌘ + ⇧ + ↩	Insert line before
⌘ + ⌃ + ↑	Move line/selection up
⌘ + ⌃ + ↓	Move line/selection down
⌘ + L	Select line - Repeat to select next lines
⌘ + D	Select word - Repeat to select next occurrence
⌃ + ⌘ + G	Select all occurrences of current selection
⌃ + ⇧ + ↑	Extra cursor on the line above
⌃ + ⇧ + ↓	Extra cursor on the line below
⌃ + M	Jump to closing parentheses Repeat to jump to opening parentheses
⌃ + ⇧ + M	Select all contents of the current parentheses
⌃ + A	Move to beginning of line
⌃ + E	Move to end of line
⌘ + K, ⌘ + K	Delete from cursor to end of line
⌘ + K + ⌫	Delete from cursor to start of line
⌘ + ]	Indent current line(s)
⌘ + [	Un-indent current line(s)
⌘ + ⇧ + D	Duplicate line(s)
⌘ + J	Join line below to the end of the current line
⌘ + /	Comment/un-comment current line
⌘ + ⌥ + /	Block comment current selection
⌘ + Y	Redo, or repeat last keyboard shortcut command
⌘ + ⇧ + V	Paste and indent correctly
⌃ + Space	Select next auto-complete suggestion
⌃ + U	Soft undo; jumps to your last change before undoing change when repeated
⌃ + ⇧ + Up	Column selection up
⌃ + ⇧ + Down	Column selection down
⌃ + ⇧ + W	Wrap Selection in html tag
⌃ + ⇧ + K	Delete current line of cursor
Navigation/Goto Anywhere
Keypress	Command
⌘ + P or ⌘ + T	Quick-open files by name
⌘ + R	Goto symbol
 	Goto word in current file
⌃ + G	Goto line in current file
General
Keypress	Command
⌘ + ⇧ + P	Command Palette
⌃ + `	Python Console
⌃ + ⌘ + F	Toggle fullscreen mode
⌃ + ⇧ + ⌘ + F	Toggle distraction-free mode
⌘ + K, ⌘ + B	Toggle side bar
⌃ + ⇧ + P	Show scope in status bar
Find/Replace
Keypress	Command
⌘ + F	Find
⌘ + ⌥ + F	Replace
⌘ + ⇧ + F	Find in files
Scrolling
Keypress	Command
⌃ + V	Scroll down one page
⌃ + L	Center current line vertically in page
⌘ + Down	Scroll to end of file
⌘ + Up	Scroll to start of file
Tabs
Keypress	Command
⌘ + ⇧ + t	Open last closed tab
⌘ + [NUM]	Jump to tab in current group where num is 1-9
⌘ + 0	Jump to 10th tab in current group
⌘ + ⇧ + [	Cycle left through tabs
⌘ + ⇧ + ]	Cycle right through tabs
^ + Tab	Cycle up through recent tabs
⇧ + ^ + Tab	Cycle down through recent tabs
 	Find in files
Split window
Keypress	Command
⌘ + ⌥ + 1	Revert view to single column
⌘ + ⌥ + 2	Split view into two columns
⌘ + ⌥ + 3	Split view into three columns
⌘ + ⌥ + 4	Split view into four columns
⌘ + ⌥ + 5	Set view to grid (4 groups)
⌃ + [NUM]	Jump to group where num is 1-4
⌃ + ⇧ + [NUM]	Move file to specified group where num is 1-4
Bookmarks
Keypress	Command
⌘ + F2	Toggle bookmark
F2	Next bookmark
⇧ + F2	Previous bookmark
⇧ + ⌘ + F2	Clear bookmarks
Text manipulation
Keypress	Command
⌘ + K, ⌘ + U	Transform to Uppercase
⌘ + K, ⌘ + L	Transform to Lowercase
⌘ + ⌃ + up, ⌘ + ⌃ + down	Clip text upwards / downwards
```

## 常用插件

### package control 这个插件是管理插件

Ctrl+`， 输入下面命令，就能安装好package control了。

    import urllib.request,os,hashlib; h = 'eb2297e1a458f27d836c04bb0cbaf282' + 'd0e7a3098092775ccb37ca9d6b2e4b7d'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)

然后我们按住 ctrl+shift+p。此时会输出一个输入框， 输入pcip,即可选择package contrl:install package。

在输入框中输入文字进行搜索插件进行安装。

如果要卸载插件， ctrl+shift+p 输入 pcrp 选择package control:remove package 然后再选择已安装的插件， 回车即可卸载。


### ctags

这个插件能跨文件跳转，跳转到指定函数声明的地方。需要查新安装ctags

	brew install ctags

	rebuild_ctags	ctrl+t, ctrl+r
	navigate_to_definition	ctrl+t, ctrl+t	ctrl+>	ctrl+shift+left_click
	jump_prev	ctrl+t, ctrl+b	ctrl+<	ctrl+shift+right_click
	show_symbols	alt+s
	show_symbols (all files)	alt+shift+s
	show_symbols (suffix)	ctrl+alt+shift+s

### sublimecodeintel 代码提示

sublime默认的代码提示只能提示系统函数，用户自己创建的函数、类不能提示。 如果想要提示自己建立的函数。 可以安装sublimecodeintel插件。

sublimecodeintel 安装后需要配置，文件：插件目录/.codeintel/config 中 增加

     "PHP": {
          "php": 'D:\SaeServer\php\php.exe',
          "phpExtraPaths": ['D:\SaeServer\php\stdlib'],
          "phpConfigFile": 'D:\SaeServer\apache\php.ini'
     },

配置了php执行文件的地址， php的配置文件地址， phpExtraPaths 是额外需要代码提示的类库，除了当前项目下的PHP代码可以提示外 phpExtraPaths中定义的目录下的PHP代码也能提示。D:\SaeServer\php\stdlib 是SaeServer中 SAE本地模拟文件的目录， 所以配置后不管在哪儿 都能有SAE代码的提示。

安装sublimecodeintel后， 按alt+鼠标左键也能和ctags一样跳转到函数声明的地方。 但是如果有两个文件声明了同样名称的函数， sublimecodeintel只会跳转到第一个找到的函数， 而ctags会让你选择要跳转到哪个文件。所以我们一般还是用ctags的跳转功能。

### 语法提示

我们需要在写代码的时候如果有语法错误，能立即提示我们， 可以安装这两个插件：sublimelint 和Phpcs ， sublimeint 需要系统有php命令。 所以需要设置好php的环境变量。 sublimelint的语法错误提示是显示在状态栏上面的，所以在编写程序的时候注意时常看看状态栏。 而Phpcs的语法错误提示是在我们保存文件时弹出万能面板显示错误，sublimelint的错误提示实时但不明显。 Phpcs的错误提示不是实时的，但很明显。 因此我们一般这两个插件都要安装。Phpcs除了代码提示的共，还有其他功能，但是我暂时没有弄明白其他功能怎么用， 大家可以去研究一下，如果知道怎么用了再告诉我一下。

### goto document

这个插件能帮助我们快速查看手册。 比如我们在写php代码时， 突然忘记了某个函数怎么用了，将鼠标放在这个函数上，然后按F1，它能快速打开PHP手册中说明这个函数用法的地方。
 安装好 goto document插件后我们再配置快捷键F1 跳转到文档。 打开sublime的菜单栏Preferences->key bindings -User设置快捷键：
     [
          { "keys": ["f1"], "command": "goto_documentation" }
     ]

这样设置后， 按F1就能跳转到文档了。

### function name display

这个插件可以在状态栏显示出当前光标处于哪个函数中。

### GBK Encoding Support

sublime本身不支持GBK编码， 可以安装这个插件让它支持。

### SVN插件

windows下可以安装Tortoise和 Tortoisesvn的客户端。然后在sublime中在目录或文件右键都可以提交svn了。 在ubuntu下可以安装rabbitvcs 结合这个插件：https://github.com/kervin/sublime-svn/downloads 实现同样的功能。

### gist

我们建立html文件时，做有些相同的代码。 这时候我们喜欢能有一个代码模板， 不能写重复相同的代码， gits插件能实现代码模板的功能。 它能见我们自己创建的代码模板，代码片段保持在github中的gist下。 http://lucifr.com/2012/03/07/sub ... al-snippet-manager/ 这里介绍了详细的用法。

### 代码注释格式化

additional PHP snippet插件能提示phpdocument格式的代码

还能快速输出开源协议， 输入php- 会有提示

安装DocBlockr 插件，能形成注释块。不用每次敲注释的斜杠或星号。

### 成对匹配的增强

像这些符号是成对的：花括号{}， 中括号[],括号：() ，引号“” 等。 这些符号当我们鼠标放在开始符号的位置的时候， 希望能明显看到结尾符号在哪儿sublime默认是下划线，很不明显， 想要明显一点，可以安装插件BracketHighlighter。

### 格式化PHP代码

安装 php-beautifier 插件，使用php-beautifier还需要安装 PHP Beutifier的pear包：
pear install PHP_Beautifier
安装好后， 打开PHP文件,ctrl+alt+f 就能为你自动格式化代码。

### Xdebug

可以安装xdebug插件，做代码调试功能。 这是大型IDE都有的功能， 小型编辑器很少能做到，但是sublime却又相应的插件能实现xdebug的功能。

你的PHP需要安装xdebug扩展。使用时需要在项目目录下建立一个.sublime-project文件

     {
          "folders":
          [
               {
                    "path": "D:\ysd\ysdv8"
               },
          ],
          "settings": {
               "xdebug": { "url": "http://yunshangdian.com" }
          }
     }

path配置项配置了程序所在路径。
注意给程序设置断点。否则不能见效果。 详细用法见：https://github.com/Kindari/SublimeXdebug

你如果也要写前端代码， 还可以安装一些和html，js相关的插件。如 ZenCoding，jQuery，jQuery Mobile Snippets，jQuery Snippets pack等。