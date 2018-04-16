PHP语法检查


我们在php编码的时常常需要对代码进行检查，它自带php -l功能太弱了，只能针对语法错误进行检查。 我需要的是一种能在生产环境中使用的检查工具，比如要有检测无用变量，或者直接使用了未经初始化的变量，当然还要能分析出潜在的错误代码，最好还能够检测出一些代码风格问题。这样可以在较大规模开发时，控制部分质量问题。
比如Lint这一工具集，它除了最初的c语言版以外，还有其它版本的实现CSS Lint, JS Lint等等，不知道php有没有类似的工具。


PHP Mess Detector(http://phpmd.org/)
PHP项目体检工具，根据你设定的标准（如单一文件代码体积，未使用的参数个数，未使用的方法数）检查PHP代码，超出设定的标准时报警。
PHP Copy Paste Detector(https://github.com/sebastianbergmann/...)
顾名思义，检查冗余代码的
PHP Dead Code Detector(https://github.com/sebastianbergmann/...)
看名字就知道了，检查从未被调用过的方法
PHP Code Sniffer(http://pear.php.net/package/PHP_CodeS...)
老牌代码格式化工具，PHP写的，Pear包，可自己hack，可集成到命令行里。我一直用的PHP Code Beautifier，只有Windows GUI，Windows CMD很难用，已经打算跳槽到PHP CS了
你还可以用jenkins把上述工具以plugins形式整合起来，做持续集成：http://jenkins-php.org/
你还可以用xinc+phing跟上述工具集成起来做持续集成后的自动化打包发布：http://code.google.com/p/xinc/

PHPLint http://www.icosaedro.it/phplint/

https://github.com/FriendsOfPHP/PHP-CS-Fixer#usage

PHPCheckstyle的官方網站為 http://code.google.com/p/phpcheckstyle/


PHP Parser
PHPSandbox
PHP Mess Detector
PHPCPD
PHPCheckstyle
Ubench
PHP Analyzer
http://www.admin10000.com/document/6257.html



find ./ -name "*.php" -exec php phpcs.phar --report=summary {} \;