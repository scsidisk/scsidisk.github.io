Homebrew 的使用
==============

### 安装

参见 http://brew.sh

### 基本用法

-   brew search formula \# 搜索软件包
-   brew install formula \# 安装软件包
-   brew remove formula \# 移除软件包
-   brew cleanup formula \# 清除旧包
-   brew list \# 列出已安装的软件包
-   brew update \# 更新 Homebrew
-   brew upgrade \# 升级软件包
-   brew home formula \# 用浏览器打开
-   brew info formula \# 显示软件内容信息
-   brew deps formula \# 显示包的依赖
-   brew server \# 启动 web 服务器，可以通过浏览器访问
    http://localhost:4567 来通过网页来管理包
-   brew -h \# 帮助
-   brew versions formula \# 列出软件包的版本

具体的用法可以 `man brew`。

### HOMEBREW_GITHUB_API_TOKEN 问题

如下出现下面提示

    Error: GitHub API rate limit exceeded for xxx.xxx.xxx.xxx (But here's the good news: Authenticated requests get a higher rate limit. Check out the documentation for more details.)
    Try again in x minutes x seconds, or create a personal access token:
      https://github.com/settings/tokens/new?scopes=&description=Homebrew
    and then set the token as: HOMEBREW_GITHUB_API_TOKEN


访问 https://github.com/settings/tokens/new?scopes=&description=Homebrew，点击 Generate token

复制刚生成的token，添加到 `~/.bash_profile`

    if [ -f /usr/local/bin/brew ]; then
        export HOMEBREW_GITHUB_API_TOKEN=xxxxxxxxxx
    fi

最后 `. ~/.bash_profile` 更新下你的环境变量。

### 多版本共存

另外一个，安装不同版本的 formula 的技巧，比方要安装不同版本的 htop

    $ brew versions htop                        # 显示 htop 的历史版本
    Warning: brew-versions is unsupported and may be removed soon.
    Please use the homebrew-versions tap instead:
      https://github.com/Homebrew/homebrew-versions
      0.8.2.2  git checkout 43b7f5b Library/Formula/htop-osx.rb
      0.8.2.1  git checkout 8e24412 Library/Formula/htop-osx.rb
      2012-0   git checkout 174acd6 Library/Formula/htop-osx.rb
      0.8.2.1-2012-04-18 git checkout 258c649 Library/Formula/htop-osx.rb
    ......
    ......
    $ cd /usr/local
    $ git checkout -b htop0.8.2.1 8e24412       # checkout 出历史版本
    Switched to a new branch 'htop0.8.2.1'
    ^_^ /usr/local $ git branch
    * htop0.8.2.1
      master
    $ brew unlink htop
    Unlinking /usr/local/Cellar/htop-osx/0.8.2.2... 4 links removed
    ......
    ......
    ^_^ /usr/local $ brew install htop          # 安装旧版本
    ==> Installing htop-osx
    ==> Downloading https://github.com/max-horvath/htop-osx/archive/0.8.2.1-2013-03-31.tar.gz
    ......
    ......
    ^_^ /usr/local $ ls /usr/local/Cellar/htop-osx/
    0.8.2.1 0.8.2
    ......
    ......
    ^_^ /usr/local $ htop --version
    htop 0.8.2.1 - (C) 2004-2008 Hisham Muhammad.
    Released under the GNU GPL.

    ^_^ /usr/local $ brew switch htop 0.8.2.2   # 切换 htop 版本
    Cleaning /usr/local/Cellar/htop-osx/0.8.2.1
    Cleaning /usr/local/Cellar/htop-osx/0.8.2.2
    4 links created for /usr/local/Cellar/htop-osx/0.8.2.2
    ^_^ /usr/local $ htop --version
    htop 0.8.2.2 - (C) 2004-2008 Hisham Muhammad.
    Released under the GNU GPL.
    ......
    ......
    ^_^ /usr/local $ brew switch htop 0.8.2.1   # 删除某版本的话，先版本切换，再删除，最后版本切换回来
    Cleaning /usr/local/Cellar/htop-osx/0.8.2.1
    4 links created for /usr/local/Cellar/htop-osx/0.8.2.1
    ^_^ /usr/local $ brew remove htop
    Uninstalling /usr/local/Cellar/htop-osx/0.8.2.1...


其实就是 Homebrew 对 git 分支的灵活应用。

### 删除 formula 时删除依赖

    $ brew remove formula
    $ brew rm $(join <(brew leaves) <(brew deps formula))

### Homebrew Cask

通过 homebrew cask 来安装 app，首先是安装

    $ brew tap caskroom/cask && brew install brew-cask

默认 homebrew cask 安装 app 至 `/opt/homebrew-cask/Caskroom` 下，并链接到 `~/Applications` 目录。

配置下，让 app 链接至 `~/My Applications` 下

    $ mkdir ~/My\ Applications
    $ touch ~/My\ Applications/.localized
    $ echo 'export HOMEBREW_CASK_OPTS="--appdir=~/My\ Applications"' >> ~/.bash_profile
    $ source ~/.bash_profile

然后随便安装了

    $ brew cask search firefox
    $ brew cask install firefox

转自：//havee.me/mac/2013-12/how-to-install-and-use-homebrew.html
