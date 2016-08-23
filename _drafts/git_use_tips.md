git 使用技巧
============


### git-svn 版本号签出

    git svn clone -r31097:HEAD {repo-url} --username={username}

### git use difffork

安装 DiffFork 后需要设置后才能结合git

1. 安装 DiffFork 命令行工具

2. 创建git比较文件

        vi /usr/local/bin/gitdfdiff.sh

        #!/bin/sh
        difffork "$2" "$5" -w

        chmod u+x /usr/local/bin/gitdfdiff.sh

3. 现在可以使用 DiffFork 进行比较

        git diff

### Git 命令行自动补全

在Pro Git上看到的技巧，git的源代码包里的 contrib/completion 目录下有个 git-completion.bash，把这个文件保存到 ~/.git-completion.bash ，然后在 ~/.bash_profile 中加入一行

source ~/.git-completion.bash

这样就能在bash下用tab自动补全git命令、branch等内容了。另外Debian/Ubuntu里有个包就叫git-completion，这个包安装完成后会自动把这个补全脚本放到/etc/bash_completion.d/下，由bash-compleletion载入执行。

