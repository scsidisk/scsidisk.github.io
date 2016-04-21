PHP开发环境 - Git(代码仓库)
========================

环境
----

- Mac OS X 10.10

安装
----

Mac OS X 会预装 git，检查 git 是否已经安装。

	$ git --version

如果没有，可以使用 Brew 进行安装

	$ brew install git
使用
----

	初始化 git 仓库
	$ git init
	检出/拉取 git 仓库
	$ git clone username@host:/path/to/repository
	$ git clone host:/path/to/repository
	$ git clone /path/to/repository
	添加单个文件
	$ git add <filename>
    添加所有文件
	$ git add .
    添加所有改动的文件
    $ git add -u
    提交改动
    $ git commit -m "代码提交信息"
    推送本地的 master 分支到远程
    $ git push origin master
    添加远程仓库
    $ git remote add origin <server>
    创建分支
    $ git checkout -b feature_x
    切换分支
    $ git checkout master
    删除分支
    $ git branch -d feature_x
    拉取/合并代码
    $ git pull
    合并其他分支到当前分支
    $ git merge <branch>
    打标签
    $ git tag 1.0.0 1b2e1d63ff
    查看提交日志
    $ git log
    放弃单个文件改动
    $ git checkout -- <filename>
    放弃本地所有文件，注意已经提交到远程的提交不要重置
    $ git reset --hard origin/master

使用技巧
-------

### 缓存本地修改

如果想把当前的修改缓存起来，进行其他操作，然后再恢复当前的工作

	$ git stash        ## 丢进暂存区
	$ git stash pop    ## 取出最后缓存内容, 并移除.

### 查看文件所有修改

	$ git blame filename

### 丢弃历史

丢弃该分支多次提交的历史，作为一次提交

	$ git merge --squash <branch>

### 理顺合并历史

如果希望单步提交历史，不要交叉，可以使用 rebase

> 不要 rebase 已经 push 的代码。

	$ git rebase <hash>
	$ git rebase -i <hash>

使用 -i 参数可以在交互处理，你可以合并提交，编辑信息，丢弃提交等等。

    p, pick：正常选中
    r, reword：选中，并且修改提交信息；
    e, edit：选中，rebase时会暂停，允许你修改这个commit（参考这里）
    s, squash：选中，会将当前commit与上一个commit合并
    f, fixup：与squash相同，但不会保存当前commit的提交信息
    x, exec：执行其他shell命令

git push命令要加上force参数，因为rebase以后，分支历史改变了，跟远程分支不一定兼容，有可能要强行推送

    $ git push --force origin myfeature

图形化工具
---------

- GitX (L) 可以使用 `brew cask install rowanj-gitx` 安装
- GitHub Desktop 管理工具，github出品，`brew cask install github-desktop` 安装
- Source Tree 管理工具，Atlassian出品，`brew cask install sourcetree` 安装

Terminal 分支提示
----------------

如果你使用终端操作 git ，下面的方法可以在 bash 中显示当前的分支。

    $ vim ~/.bash_profile

添加下面内容

    # Git branch in prompt.
    parse_git_branch() {
        git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
    }
    export PS1="\h:\W \u\[\033[32m\]\$(parse_git_branch)\[\033[00m\] $ "

命令行自动补全
-------------

下载 git 版本库里面的 [git-completion.bash](https://raw.github.com/git/git/master/contrib/completion/git-completion.bash)，把这个文件保存到~/.git-completion.bash，然后在.bashrc中加入一行

    if [ -f ~/.git-completion.bash ]; then
      . ~/.git-completion.bash
    fi

这样就能在bash下用tab自动补全 git 命令、 branch 等内容了。另外 Debian/Ubuntu 里有个包就叫 git-completion ，这个包安装完成后会自动把这个补全脚本放到 /etc/bash_completion.d/ 下，由 bash-compleletion 载入执行。

git 工作流
---------

### 三种工作流

目前使用比较多的是 Git flow， Github flow， Gitlab flow。

Git flow 工作流程清晰可控，但是相对复杂。针对版本发布，需要同时维护两个长期分支。参考 [Git flow 项目](https://github.com/nvie/gitflow).

Github flow 专门配合"持续发布"。一般项目只有 master 分支不够用，需要另外开 production 分支跟踪线上版本。

Gitlab flow 是 Git flow 与 Github flow 的综合。它的最大原则叫做"上游优先"（upsteam first），即只存在一个主分支master，它是所有其他分支的"上游"。只有上游分支采纳的代码变化，才能应用到其他分支。

### Gitlab flow 工作流（推荐）

对于"持续发布"的项目，它建议在 master 分支以外，再建立不同的环境分支。比如，master 作为"开发环境" ，"预发环境"的分支是 pre-production ，"生产环境"的分支是 production 。

代码的变化，必须由 master -> pre-production -> production .

对于"版本发布"的项目，建议每一个稳定版本，都要从 master 开一个分支，比如 2.3 、 2.4 等等。 并可设置默认分支。

以后，只有修补bug，才允许将代码合并到这些分支。

### Git flow 工作流

Git flow 提供了极出色的命令帮忙以及输出提示。基于归并的解决方案，它并没有提供重置(rebase)特性分支的能力。

需要一个 production releases 分支，默认为 master，

可以使用其他名字，如 RELEASE 分支，然后使用 master 分支作为 develop 分支。

Sourcetree 已经提供了 git-flow 的支持。

	$ brew install git-flow               ## 安装
	$ git flow init                       ## 初始化

新特性

	$ git flow feature start MYFEATURE    ## 增加新特性，基于'develop'
	$ git flow feature finish MYFEATURE   ## 完成新特性，合并并切换到 'develop',
	$ git flow feature publish MYFEATURE  ## 发布新特性
	$ git flow feature pull MYFEATURE     ## 拉取新特性

realse

	$ git flow release start RELEASE [HASH]  ## 从develop创建release
	$ git flow release publish RELEASE    ## 发布release
	$ git flow release track RELEASE      ## 拉取release
	$ git flow release finish RELEASE     ## 完成release到master

紧急修复补丁

	$ git flow hotfix start VERSION [HASH]   ## 创建补丁
	$ git flow hotfix finish VERSION      ## 完成紧急修复到master

现在分支管理不麻烦了。


提交注释格式
----------

Git 每次提交代码，都要写 Commit message（提交说明），否则就不允许提交。

commit message 应该清晰明了，说明本次提交的目的。

### Commit message 的格式

每次提交，Commit message 都包括三个部分：Header，Body 和 Footer。

	<type>(<scope>): <subject>
	// 空一行
	<body>
	// 空一行
	<footer>

其中，Header 是必需的，Body 和 Footer 可以省略。

不管是哪一个部分，任何一行都不得超过72个字符（或100个字符）。这是为了避免自动换行影响美观。

#### Header

Header部分只有一行，包括三个字段：type（必需）、scope（可选）和subject（必需）。

（1）type

type用于说明 commit 的类别，只允许使用下面7个标识。

	feat：新功能（feature）
	fix：修补bug
	docs：文档（documentation）
	style： 格式（不影响代码运行的变动）
	refactor：重构（即不是新增功能，也不是修改bug的代码变动）
	test：增加测试
	chore：构建过程或辅助工具的变动

如果type为feat和fix，则该 commit 将肯定出现在 Change log 之中。其他情况（docs、chore、style、refactor、test）由你决定，要不要放入 Change log，建议是不要。

（2）scope

scope用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。

（3）subject

subject是 commit 目的的简短描述，不超过50个字符。

- 以动词开头，使用第一人称现在时，比如change，而不是changed或changes
- 第一个字母小写
- 结尾不加句号（.）

#### Body

Body 部分是对本次 commit 的详细描述，可以分成多行。下面是一个范例。

	More detailed explanatory text, if necessary.  Wrap it to
	about 72 characters or so.

	Further paragraphs come after blank lines.

	- Bullet points are okay, too
	- Use a hanging indent

有两个注意点。

- 使用第一人称现在时，比如使用change而不是changed或changes。
- 应该说明代码变动的动机，以及与以前行为的对比。

### Footer

Footer 部分只用于两种情况。

（1）不兼容变动

如果当前代码与上一个版本不兼容，则 Footer 部分以BREAKING CHANGE开头，后面是对变动的描述、以及变动理由和迁移方法。

	BREAKING CHANGE: isolate scope bindings definition has changed.

	    To migrate the code follow the example below:

	    Before:

	    scope: {
	      myAttr: 'attribute',
	    }

	    After:

	    scope: {
	      myAttr: '@',
	    }

	    The removed `inject` wasn't generaly useful for directives so there should be no code using it.

（2）关闭 Issue

如果当前 commit 针对某个issue，那么可以在 Footer 部分关闭这个 issue 。

	Closes #234

也可以一次关闭多个 issue 。

	Closes #123, #245, #992

#### Revert

还有一种特殊情况，如果当前 commit 用于撤销以前的 commit，则必须以revert:开头，后面跟着被撤销 Commit 的 Header。

	revert: feat(pencil): add 'graphiteWidth' option

	This reverts commit 667ecc1654a317a13331b17617d973392f415f02.

Body部分的格式是固定的，必须写成 `This reverts commit <hash>`.，其中的hash是被撤销 commit 的 SHA 标识符。

如果当前 commit 与被撤销的 commit，在同一个发布（release）里面，那么它们都不会出现在 Change log 里面。如果两者在不同的发布，那么当前 commit，会出现在 Change log 的Reverts小标题下面。

### 安装 commitizen

Commitizen是一个撰写合格 Commit message 的工具。

如果不是 node 项目，需要在项目根目录执行 npm init 创建 package.json

	## 全局安装 commitizen
	$ npm install commitizen -g
	## 初始化环境支持 commitizen
	$ npm install jasmine --save-dev
	$ commitizen init cz-conventional-changelog \
	--save --save-exact

以后 `git commit -m "msg"` 换成使用 `git cz` 即可安装 angularjs 推荐的格式进行提交。

### 检查注释格式正确性

	## 安装提交信息验证
	$ npm install ghooks --save-dev
	$ npm install validate-commit-msg  --save-dev
	$ vim package.json

添加下面内容

	"config": {
	  "ghooks": {
	    "commit-msg": "./node_modules/validate-commit-msg/index.js"
	  }
	}

现在，使用 `git commit -m "something"` 会提示格式有问题。

### 查询提交

规范的提交信息可以更方便查询

	## 方便浏览
	$ git log <last tag> HEAD --pretty=format:%s
	## 方便过滤
	$ git log <last release> HEAD --grep feature

### 生成 Change log

如果你的所有 Commit 都符合 Angular 格式，那么发布新版本时， Change log 就可以用脚本自动生成。

生成的文档包括以下三个部分。

- New features
- Bug fixes
- Breaking changes.

每个部分都会罗列相关的 commit ，并且有指向这些 commit 的链接。当然，生成的文档允许手动修改，所以发布前，你还可以添加其他内容。

conventional-changelog 就是生成 Change log 的工具， 下面安装

	$ npm install -g conventional-changelog
	$ cd my-project
	## 生成最新的 changelog
	$ conventional-changelog -p angular -i CHANGELOG.md -w
	## 生成所有的 changelog
	$ conventional-changelog -p angular -i CHANGELOG.md -w -r 0

编辑 package.json，添加

	{
	  "scripts": {
	    "changelog": "conventional-changelog -p angular -i CHANGELOG.md -w -r 0"
	  }
	}

以后，执行下面命令即可重新生成所有 changelog

	$ npm run changelog

参考
----

- <http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html>
- <https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E9%87%8D%E5%86%99%E5%8E%86%E5%8F%B2>
- <http://www.ruanyifeng.com/blog/2015/12/git-workflow.html>