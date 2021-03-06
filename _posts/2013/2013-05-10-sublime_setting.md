---
layout: post
title: "sublime配置全攻略"
date: 2013-05-10 13:25:57
author: scsidisk
categories: Sublime
tags: Mac, Sublime
---

大家好，今天给大家分享一款编辑器：sublime text2

我用过很多编辑器，EditPlus、EmEditor、Notepad++、Notepad2、UltraEdit、Editra、Vim，还有包括netbeans , zendstudio, dreamweaver 等。 最后我遇见了sublime text。sublime是我见过的最好的编辑器，大型IDE能实现的功能， 用sublime装上相应插件，都能实现。 它是一个小型编辑器， 运行速度很快。现在是鼓起勇气换掉你以前编辑器的时候了。

sublime本身功能有限，我们需要装上一些插件使其变得强大。sublime在各个操作系统下都可以运行，但在linux下运行需要注意中文输入的问题。 下面我主要介绍一下常用插件、配置的建议以及在linux下运行的注意事项。

常用插件
--------

### package control。

我们用sublime几乎都会首先安装这个插件，这个插件是管理插件的功能，先安装它，再安装其他插件就方便了。安装方法：

点击sublime的菜单栏 view->show console ；现在打开了控制台， 这个控制台有上下两栏， 上面一栏会实时显示sublime执行了什么插件，输出执行结果， 如果你安装的某个插件不能正常运行，应该先在这里看看有没有报错。下面栏是一个输入框，可以运行python代码。我们输入下面的代码点击回车运行， 就能安装好package control了。

    import urllib2,os;pf='Package Control.sublime-package';\
    ipp=sublime.installed_packages_path();os.makedirs(ipp) \
    if not os.path.exists(ipp) else None;\
    open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen(\
    'http://sublime.wbond.net/'+pf.replace(' ','%20')).read())

然后我们按住 ctrl+shift+p。此时会输出一个输入框， 输入install。选择package contrl： install package 回车 ，需要稍定一会儿，右下角状态栏会显示正在连接的提示文字。 使用sublime时注意看右下角状态栏，很多插件的提示信息都显示在这里，这个状态栏很小，初次使用的人都有可能没有注意到它。

稍等一会儿后，它会出现一个插件列表， 你也可以在输入框中输入文字进行搜索插件。 搜索到自己想安装的插件，再选择它，回车。 就自动给你安装好了。

如果要卸载插件， ctrl+shift+p 输入 remove， 选择package control:remove package 然后再选择已安装的插件， 回车即可卸载。

如果package control 安装插件时失败了， 我们可以采用手动安装的方式， 在google上去搜索插件， 下载插件的源代码。在sublime的菜单栏点击 preferences->Browse package..此时会打开插件目录。然后把你下载的插件源代码复制进去就可以了。

    ctrl+shift+p 打开的输入框面板是什么？ 英文叫做 “Anything panel” ，任何操作都可以在这个面板里面完成。我暂且翻译为“万能面板”。 打开万能面板有几种方式。
    ctrl+shift+p 打开时，我们需要在面板中输入一个命令，然后执行命令。所有菜单栏能操作事都可以在这里输入命令进行操作。
    ctrl+p 打开时，能快速查找文件。
    ctrl+r 打开时， 能查找当前文件中的函数。
    ctrl+g 打开时，能跳转到指定行。

大家开始接触sublime时对它的环境还不是很熟悉，所有我在这里说得有点多， 简单总结一下前面说的。

控制台的作用： 可以在这里执行python代码，和查看一些执行结果，如果插件运行不正常，可以在这里看看有没有报错。
右下角状态栏： 很多提示信息都会显示在那里，注意经常查看。

万能面板：所有的操作都可以在这里进行，又可以在这里输入命令，又可以在这里查找文件，也可以在这里查找函数等等。
安装插件的方式：除了package control 安装还可以手动安装。

### ctags

这个插件能跨文件跳转，跳转到指定函数声明的地方。 使用package control 搜索ctags 进行安装（安装ctags插件就可以了， 还有一个 CTags for PHP 插件没什么用）。

注意安装好插件后要需要安装ctags命令。window 下载 ctags.exehttp://vdisk.weibo.com/s/7QZd7 。 将ctags.exe文件放在一个环境变量能访问到的地方。打开cmd， 输入ctags，如果有这个命令，证明成功了。

ubuntu下安装运行命令：sudo apt-get install exuberant-ctags 。
 然后在sublime项目文件夹右键， 会出现Ctag:Rebuild Tags 的菜单。点击它，然后会生成.tags的文件。

然后在你代码中， 光标放在某个函数上， 点击ctrl+shift+鼠标左键 就可以跳转到函数声明的地方。

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

### TrailingSpacer

高亮显示多余的空格和Tab

在github上下载的插件缺少了一个设置快捷键的文件，可以新建一个名字和后缀为Default (Windows).sublime-keymap的文件，添加以下代码，即可设置“删除多余空格”和“是否开启TrailingSpacer ”的快捷键了。


    [
        { "keys": ["ctrl+alt+d"], "command": "delete_trailing_spaces" },
        { "keys": ["ctrl+alt+o"], "command": "toggle_trailing_spaces" }
    ]

### Alignment 等号对齐

按Ctrl+Alt+A，可以将凌乱的代码以等号为准左右对其，适合有代码洁癖的朋友。

.Default (OSX).sublime-keymap

    { "keys": ["super+ctrl+a"], "command": "alignment" }

### gbk4subl 支持GBK编码

让sublime text 3支持GBK编码的插件

### SideBarEnhancements

侧边栏增强插件

### CSScomb js

属性排序

### ColorPicker

调色盘,快捷键(osx) cmd+shift+c

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

配置建议
----------

### 用户配置建议

打开菜单栏Preferences->Setting-user：

    {
        "color_scheme": "Packages/Color Scheme - Default/Monokai.tmTheme",
        "default_line_ending": "unix",
        "detect_slow_plugins": false,
        "font_face": "Microsoft YaHei",
        "font_size": 10.0,
        "auto_match_enabled": false,
    }

auto_match_enabled设置为false后可以关闭括号的自动完成。如我们输入左括号时sublime自动将右括号打出来了，往往我们不习惯这样， 此时你设置auto_match_enabled为false即可。

### 快捷键配置的建议

菜单栏Preferences->key bindings -User：

    [
        { "keys": ["f1"], "command": "goto_documentation" },
        { "keys": ["alt+shift+`"], "command": "clone_file" }
    ]

F1快速打开文档， 这个快捷键的设置前面已经说了。
alt+shift+` 快捷键又有什么用呢？ 我们需要同一个文件在左右两栏同时打开。

先按快捷键： alt+shift+2 。 此时会出现左右两栏的布局。

再按alt+shift+`（`键是tab键上面个键）, 此时会复制一份当前文件， 再把新复制的那份文件拖动到右栏。 这样就实现了同一文件左右两栏同时打开了。

切换回一栏布局，按 alt+shift+1

### 颜色配置建议：

sublime对无效（invalid）的颜色提示 往往会提示错误。颜色很难看。 可以去掉对invalid的颜色提示。
插件目录下\Color Scheme - Default\Monokai.tmTheme文件中， 删除

    <dict>
        <key>name</key>
        <string>Invalid</string>
        <key>scope</key>
        <string>invalid</string>
        <key>settings</key>
        <dict>
            <key>background</key>
            <string>#F92672</string>
            <key>fontStyle</key>
            <string></string>
            <key>foreground</key>
            <string>#F8F8F0</string>
        </dict>
    </dict>

成对匹配默认是绿色，有点难看，

插件目录下\Color Scheme - Default\Monokai.tmTheme文件中Class name 键中的：
改为：

    <dict>
        <key>name</key>
        <string>Class name</string>
        <key>scope</key>
        <string>entity.name.class</string>
        <key>settings</key>
        <dict>
            <key>background</key>
            <string>#F92672</string>
            <key>fontStyle</key>
            <string></string>
            <key>foreground</key>
            <string>#F8F8F0</string>
        </dict>
    </dict>

在linux下使用。
--------------

linux下使用时，中文不能输入的问题， 使用scim输入法方式可以解决。具体解决方法：

http://www.haogongju.net/art/1312281[]

虽然scim能让我们输入中文后， 但是也不是很完美，有候选词不跟随的问题， sublime失焦后候选词会消失的问题。候选词消失的问题，可以把sublime独立到一个单独的工作区中来暂时解决这个问题。