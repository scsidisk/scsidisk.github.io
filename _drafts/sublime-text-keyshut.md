Sublime Text 快捷键

### 快捷键

line相关：
Ctrl+Shift+D：复制当前行
Ctrl+Shift+K：删除当前行
Ctrl+j 合并一行
Ctrl+Enter：在当前行下添加新行。After
Ctrl+Shift+Enter：在当前行上添加新行。Before

Comment注释：
Ctrl+/：行注释。
Ctrl+Shift+/：块注释

Ctrl+Shift+P：调用命令面板，快速查找，例如：改变语法模式等。
模糊匹配，可以减少对快捷键的记忆。

Shift+Alt+1，2，3，4，5：开启对应数字的多栏编辑

Ctrl+P：Goto Anything

Ctrl+P: 查找项目中的文件：
直接输入名称：在不同文件中切换，支持级联的目录模式
:：+ 行号：Ctrl+G 定位到具体的行。
@：+ 符号：Ctrl+R定位到具体的符号，例如：JS函数名，CSS选择器名。
#：+ 关键字：Ctrl+;匹配到具体的匹配的关键字。主要是模糊匹配。

多行游标
Ctrl+D：选中当前光标所在位置的单词。连续使用时，进行多光标选择，选中下一个同名单词。
Ctrl+K：配合Ctrl+D可以跳过下一个同名单词。
Ctrl+L：选择当前光标所在位置的行。连续使用时，继续选中下一行。
Ctrl+Shift+L：在多行选中后，在所有选中的行后产生游标。
Alt+F3：选中文档中所有的同名单词。
Shift+鼠标右键：向下拖动，产生多个光标。


### 设置

Settings-User
// User/Preferences.sublime-settings
//我觉得自带字体挺好的。
{
    "color_scheme": "Packages/User/SublimeLinter/Dawn (SL).tmTheme", //主题
    "draw_minimap_border": true, // 右侧缩略图边框
    "font_size": 10, // 字体大小
    "highlight_line": true, // 当前行标亮
    "save_on_focus_lost": true, // 当前行标亮
    "theme": "Spacegray Eighties.sublime-theme", //主题相关
    "word_separators": "./\\()\"':,.;<>~!@#$%^&*|+=[]{}`~?", // 双击选中中划线
    "word_wrap": true, //自动换行
    "trim_trailing_white_space_on_save": true, //自动移除行尾多余空格
    "ensure_newline_at_eof_on_save": true, //文件末尾自动保留一个空行
    "disable_tab_abbreviations": true, //禁用 Emmet 的 tab 键功能（请使用 ctrl+e）
    "translate_tabs_to_spaces": true, //把代码 tab 对齐转换为空格对齐
    "tab_size": 4, //空格数
    "fade_fold_buttons": false, //显示代码块的倒三角
    "bold_folder_labels": true, //侧边栏文件夹加粗
    "auto_find_in_selection": true //开启选中范围内搜索
}


key - Bindings-User：个人对于快捷键的设置。同样会覆盖默认的设置。例如：

//自动改变缩进格式
{ "keys": ["shift+tab"], "command": "reindent","args":{"single_line":false} }

tools:工具下的Build System选择新建一个选项后，进行如下设置（注意后缀），保存到user目录下： 
//这样设置。。地址是你的浏览器位置
{
  "cmd" :["C:\\Program Files (x86)\\Google\\Chrome\\Application\\chrome.exe","$file"],
  "selector":["text.html"]
}

而且选择第一个auto， 修改内容后按Ctrl+B，可以看到自动打开了chrome并且是修改后的内容。