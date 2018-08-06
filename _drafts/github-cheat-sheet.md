少有人知的 GitHub 使用技巧
========================

[GitHub](https://github.com) 大家常上吧？可是使用 GitHub
的各种小窍门你就不一定知道了。本文将各种使用 GitHub 的小窍门分享给大家。

diff时忽略空格
--------------

有些修改只是增减了空格，在URL中添加`?w=1`就可以忽略。

查看某个作者的提交历史
----------------------

在URL中添加`?author=username`，例如：

    https://github.com/rails/rails/commits/master?author=dhh

比较版本
--------

使用类似如下的URL比较分支：

    https://github.com/rails/rails/compare/master...4-1-stable

同样可以使用一下格式：

    https://github.com/rails/rails/compare/master@{1.day.ago}...master
    https://github.com/rails/rails/compare/master@{2014-10-04}...master

如果想和派生仓库比较，加上派生仓库名作前缀即可：

    https://github.com/rails/rails/compare/byroot:master...master

通过 HTML 方式嵌入 Gist
-----------------------

[Gists](https://gist.github.com/)是 GitHub 推出的基于 Git
的代码片段服务。Gists页面提供JavaScript代码，可以将 Gist
嵌入到其他站点。但是很多站点粘贴 JavaScript 无效，这时候你可以在 Gist
URL 后附加`.pibb`，得到一个纯 HTML 的版本，然后就可以复制粘贴 HTML
源码到其他网站了。例如 <https://gist.github.com/tiimgreen/10545817.pibb>

Git.io
------

[Git.io](http://git.io/) 是适用于 GitHub 的短网址服务。

当然，为了~~逼格~~方便，也可以使用Curl访问：

    $ curl -i http://git.io -F "url=https://github.com/..."
    HTTP/1.1 201 Created
    Location: http://git.io/abc123

    $ curl -i http://git.io/abc123
    HTTP/1.1 302 Found
    Location: https://github.com/...

你甚至可以指定短网址的字段：

    $ curl -i http://git.io -F "url=https://github.com/technoweenie" \
        -F "code=t"
    HTTP/1.1 201 Created
    Location: http://git.io/t

高亮行
------

例如，在 URL 中加上 `#L52` 可以高亮第52行。或者你也可以直接点击行数。

多行高亮同样支持。你可以使用类似`#L53-L60`格式，或者在按住shift的同时点击。

    https://github.com/rails/rails/blob/master/activemodel/lib/active_model.rb#L53-L60

快速引用
--------

你可以选中别人的评论文字，然后按`r`，这些内容会以引用的形式被复制在文本框中：

任务列表
--------

在工单或合并请求中，你可以使用任务列表语法：

    - [ ] Be awesome
    - [ ] Do stuff
    - [ ] Sleep

勾选之后，会更新 Markdown：

    - [x] Be awesome
    - [x] Do stuff
    - [ ] Sleep

合并请求的 diff 和 patch
------------------------

可以在 URL 后添加 `.diff` 和 `.patch`，以对应的模式查看合并请求:

    https://github.com/tiimgreen/github-cheat-sheet/pull/15
    https://github.com/tiimgreen/github-cheat-sheet/pull/15.diff
    https://github.com/tiimgreen/github-cheat-sheet/pull/15.patch

结果是纯文本的：

    diff --git a/README.md b/README.md
    index 88fcf69..8614873 100644
    --- a/README.md)
    +++ b/README.md
    @@ -28,6 +28,7 @@ All the hidden and not hidden features of Git and GitHub. This cheat sheet was i
     - [Merged Branches](#merged-branches)
     - [Quick Licensing](#quick-licensing)
     - [TODO Lists](#todo-lists)
    +- [Relative Links](#relative-links)
     - [.gitconfig Recommendations](#gitconfig-recommendations)
         - [Aliases](#aliases)
         - [Auto-correct](#auto-correct)
    @@ -381,6 +382,19 @@ When they are clicked, they will be updated in the pure Markdown:
     - [ ] Sleep

    (...)

------------------------------------------------------------------------

编撰 [SegmentFault](http://sf.gg)\
 参考
[github-cheat-sheet](https://github.com/tiimgreen/github-cheat-sheet)


