使用 rbenv 安装管理 Ruby
Andor 发布于 2012年4月19日
Ruby 有不同的版本会同时存在，现在最新版本是 1.9.3-9125，但是有些开发者仍然使用 1.8.x 系列。而且很多程序只针对特定的 Ruby 版本。所以对 Ruby 做版本管理是即为重要的，这其中也就涉及到安装的问题。

常用的几个 Ruby 版本管理工具有：rvm，rbenv，ry，rbfu。rvm 应该是最早出现、使用最多的，因为过于强大以至于违背了某个 Linux 软件开发原则，所以出现了很多轻便的替代者，其中来自 37signals 的 rbenv 就很受欢迎。ry 和 rbfu 看上去更轻便，不过使用不广泛。所以我最终选择使用 rbenv。

我电脑的环境是：Mac OS X 10.6.8，XCode 3.2.3，终端是 bash。

1. 安装 rbenv ruby-build

ruby-build 这个工具用来安装编译 Ruby 源码，如果选择手动编译，可不使用这个工具。

brew install rbenv ruby-build rbenv-gem-rehash 改改启动脚本就好了, 弄起来还挺简单的, 重新安装 ruby 速度也飞快

3. 安装 Ruby

CONFIGURE_OPTS="--with-readline-dir=$(brew --prefix readline)" rbenv install -k -v 2.1.0-dev

使用 ruby-build 可以自动下载编译安装 Ruby 相应的版本，只需指定版本号。

rbenv install 1.9.3-p125
等待一大会儿，安装完毕后可以查看已经安装的 Ruby 版本：

rbenv versions
以上命令列出了安装在 rbenv 中的各 Ruby 版本，前面带有 * 号的表示是当前使用的版本。

4. 选择一个 Ruby 版本

rbenv 中的 Ruby 版本有三个不同的作用域：全局，本地，当前终端。

4.1 设置全局版本

全局版本是在没有找到“当前终端”或“本地”作用域的设置时执行。通过以下命令设置：

rbenv global 1.9.3-p125
如果要使用系统原有的 Ruby，则通过 system 指定：

rbenv global system
4.2 设置本地版本

“本地”作用域是针对各个项目的，因为不同的项目可以基于不同的 Ruby 版本开发。“本地”作用域通过项目文件夹中的 .rbenv-version 这个文件进行管理，需要将相应的 Ruby 版本号写入这个文件。这个过程可以通过以下命令执行：

rbenv local 1.9.2-p290
4.3 设置当前终端版本

“当前终端”作用域的优先级最高。通过以下命令设置：

rbenv shell 1.9.2-p290
设置完毕后可以通过以下命令进行验证：

which ruby
rbenv version
5. 安装 gem

使用 rbenv 后，gem 还是按照原有的方式进行安装、升级，只是 gem 的安装路径是在 rbenv 文件夹中当前 Ruby 版本文件夹下。而且，安装带有可执行文件的 gem 后，需要执行一个特别的命令，告诉 rbenv 更新相应的映射关系，这个命令在安装新版本的 Ruby 后也需要执行：

rbenv rehash



安装 rbenv

在 osx 上可以直接用 homebrew 安装, 下面是手动安装过程. (不用 zsh 的童鞋注意替换成自己的 shell 配置文件)

git clone git://github.com/sstephenson/rbenv.git ~/.rbenv
# 用来编译安装 ruby
git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
# 用来管理 gemset, 可选, 因为有 bundler 也没什么必要
git clone git://github.com/jamis/rbenv-gemset.git  ~/.rbenv/plugins/rbenv-gemset
# 通过 gem 命令安装完 gem 后无需手动输入 rbenv rehash 命令, 推荐
git clone git://github.com/sstephenson/rbenv-gem-rehash.git ~/.rbenv/plugins/rbenv-gem-rehash
# 通过 rbenv update 命令来更新 rbenv 以及所有插件, 推荐
git clone https://github.com/rkh/rbenv-update.git ~/.rbenv/plugins/rbenv-update
然后把下面的代码放到 ~/.bash_profile 里

export PATH="$HOME/.rbenv/bin:$PATH"
eval "$(rbenv init -)"
注意 Unubtu请放到 ~/.bashrc 里, zsh用户是 ~/.zshrc

然后重开一个终端就可以执行 rbenv 了.

使用

安装 ruby

rbenv install --list  # 列出所有 ruby 版本
rbenv install 1.9.3-p392     # 安装 1.9.3-p392
rbenv install jruby-1.7.3    # 安装 jruby-1.7.3
列出版本

rbenv versions               # 列出安装的版本
rbenv version                # 列出正在使用的版本
设置版本

rbenv global 1.9.3-p392      # 默认使用 1.9.3-p392
rbenv shell 1.9.3-p392       # 当前的 shell 使用 1.9.3-p392, 会设置一个 `RBENV_VERSION` 环境变量
rbenv local jruby-1.7.3      # 当前目录使用 jruby-1.7.3, 会生成一个 `.rbenv-version` 文件
解决 MacOSX 下编译 Ruby 无法在 irb 中输入中文的方法

安装 homebrew 的 readline，再进入源码目录，重新编译安装 readline.bundle

brew install readline
brew link readline
cd src/ruby-1.9.3-p392/ext/readline
ruby extconf.rb --with-readline-dir=$(brew --prefix readline)
make install
rbenv 下的解决办法

brew install readline
CONFIGURE_OPTS="--disable-install-doc --with-readline-dir=$(brew --prefix readline)" rbenv install 1.9.3-p392
有关 ruby-2.0.0-p0 在 OS X 10.7+ 上的问题，参见：https://github.com/sstephenson/ruby-build/wiki

其他

rbenv rehash                 # 每当切换 ruby 版本和执行 bundle install 之后必须执行这个命令
rbenv which irb              # 列出 irb 这个命令的完整路径
rbenv whence irb             # 列出包含 irb 这个命令的版本
rbenv 下使用 gemset

简介

rvm 中最方便的就是 gemset。实际上，rbenv 通过插件也可以使用 gemset

安装

MacOS 下使用 brew 的话，一个命令就搞定

brew install rbenv-gemset
使用

创建一个 gemset

rbenv gemset create 1.9.3-p392 ruby-china
                       参数 1       参数 2
以上命令中，参数 1 是已安装的 ruby 版本，参数 2 是 gemset 的名字
具体使用方法

在项目的根目录下，把想要使用的 gemset 名字放到 .rbenv-gemsets 文件中即可。有 .rbenv-gemsets 文件的情况下执行 bundle 命令就是对设置好的 gemset 进行操作
echo ruby-china > .rbenv-gemsets
当前目录下没有 .rbenv-gemsets 文件的情况下，执行 bundle 命令（没有指定 --path 参数的情况）时，是对当前版本的 ruby 版本的 gemset 。也就相当于 rvm 中 global gemset 的作用了
