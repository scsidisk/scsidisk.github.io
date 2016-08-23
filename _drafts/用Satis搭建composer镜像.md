用Satis搭建composer镜像
噜吧 叽喳:

    鉴于国内网络的情况， 想安装 composer 包都不是那么容易的事。  开源世界里， 总是有镜像 mirror 一说， 国内还没很好的 composer 镜像，pkg.phpcomposer.com 不太稳定容易出一些问题。 对于稍大点的公司， 为了方便部署多个机器，一种方式是吧 vendor 整个提交到 git/svn ， 或者选择自己本地搭建一个 composer 镜像，也不需要定期更新，镜像系统只会把已经下载过的包缓冲到私有镜像，没有的则下载一遍。 想做一个 mirror (or fake mirror)， 我所知目前有俩项目， 一个就是本文的 Satis ， 另外一个是 [Toran](https://toranproxy.com/) ， 不过它不是完全免费的开源项目，只针对个人免费， 其特点等参见官方网站 [https://toranproxy.com/](https://toranproxy.com/)
Installing all your PHP libraries with Composer is a great way to save time. But larger projects automatically tested and run at each commit to your software version control (SVC) system will take a long time to install all the required packages from the Internet. You want to run your tests as soon as possible through your continuous integration (CI) system so that you have fast feedback and quick reactions on failure. In this tutorial we will set up a local mirror to proxy all your packages required in your project's composer.json file. This will make our CI work much faster, install the packages over the local network or even hosted on the same machine, and make sure we have the specific versions of the packages always available.

What Is Satis?
Satis is the name of the application we will use to mirror the various repositories for our project. It sits as a proxy between the Internet and your composer. Our solution will create a local mirror of a few packages and instruct our composer to use it instead of the sources found on the Internet.

Here is an image that says more than a thousand words.

Architecture

Our project will use composer as usual. It will be configured to use the local Satis server as the primary source. If a package is found there, it will be installed from there. If not, we will let composer use the default packagist.org to retrieve the package.

Getting Satis
Satis is available through composer, so installing it is very simple. In the attached source code archive, you will find Satis installed in the Sources/Satis folder. First we will install composer itself.

$ curl -sS
https://getcomposer.org/installer
 | php
#!/usr/bin/env php All settings correct for using Composer Downloading...
 Composer successfully installed to: /home/csaba/Personal/Programming/NetTuts/Setting up a local mirror for Composer packages with Satis/Sources/Satis/
composer.phar
 Use it: php
composer.phar

Then we will install Satis.

$ php
composer.phar
 create-project composer/satis --stability=dev --keep-vcs Installing composer/satis (dev-master eddb78d52e8f7ea772436f2320d6625e18d5daf5)
  - Installing composer/satis (dev-master master)
    Cloning master
 Created project in /home/csaba/Personal/Programming/NetTuts/Setting up a local mirror for Composer packages with Satis/Sources/Satis/satis Loading composer repositories with package information Installing dependencies (including require-dev) from lock file
  - Installing symfony/process (dev-master 27b0fc6)
    Cloning 27b0fc645a557b2fc7bc7735cfb05505de9351be

  - Installing symfony/finder (v2.4.0-BETA1)
    Downloading: 100%

  - Installing symfony/console (dev-master f44cc6f)
    Cloning f44cc6fabdaa853335d7f54f1b86c99622db518a

  - Installing seld/jsonlint (1.1.1)
    Downloading: 100%

  - Installing justinrainbow/json-schema (1.1.0)
    Downloading: 100%

  - Installing composer/composer (dev-master f8be812)
    Cloning f8be812a496886c84918d6dd1b50db5c16da3cc3

  - Installing twig/twig (v1.14.1)
    Downloading: 100%
 symfony/console suggests installing symfony/event-dispatcher () Generating autoload files
Configuring Satis
Satis is configured by a JSON file very similar to composer. You can use whatever name you want for your file and specify it for usage later. We will use "mirrored-packages.conf".

{
    "name": "NetTuts Composer Mirror",
    "homepage": "http://localhost:4680",

    "repositories": [
        { "type": "vcs", "url": "
https://github.com/SynetoNet/monolog
" },
        { "type": "composer", "url": "
https://packagist.org
" }
    ],

    "require": {
        "monolog/monolog": "syneto-dev",
        "mockery/mockery": "*",
        "phpunit/phpunit": "*"
    },
    "require-dependencies": true
}
Let's analyze this configuration file.

name - represents a string that will be shown on the web interface of our mirror.
homepage - is the web address where our packages will be kept. This does not tell our web server to use that address and port, it is rather just information of a working configuration. We will set up the access to it on that addres and port later.
repositories - a list of repositories ordered by preference. In our example, the first repository is a Github fork of the monolog logging libraries. It has some modifications and we want to use that specific fork when installing monolog. The type of this repository is "vcs". The second repository is of type "composer". Its URL is the default packagist.org site.
require - lists the packages we want to mirror. It can represent a specific package with a specific version or branch, or any version for that matter. It uses the same syntax as your "require" or "require-dev" in yourcomposer.json.
require-dependencies - is the final option in our example. It will tell Satis to mirror not only the packages we specified in the "require" section but also all their dependencies.
To quickly try out our settings we first need to tell Satis to create the mirrors. Run this command in the folder where you installed Satis.

$ php ./satis/bin/satis build ./
mirrored-packages.conf
 ./packages-mirror Scanning packages Writing
packages.json
 Writing web view
While the process is taking place, you will see how Satis mirrors each found version of the required packages. Be patient it may take a while to build all those packages.

Satis requires that date.timezone to be specified in the php.ini file, so make sure it is and set to your local timezone. Otherwise an error will appear.

[Twig_Error_Runtime]
  An exception has been thrown during the rendering of a template
("date_default_timezone_get(): It is not safe to rely on the system's timezone settings. You are *required* to use the date.timezone setting or the date_default_timezone_set) function.
Then we can run a PHP server instance in our console pointing to the recently created repository. PHP 5.4 or newer is required.

$ php -S localhost:4680 -t ./packages-mirror/ PHP 5.4.22-pl0-gentoo Development Server started at Sun Dec  8 14:47:48 2013 Listening on http://localhost:4680 Document root is /home/csaba/Personal/Programming/NetTuts/Setting up a local mirror for Composer packages with Satis/Sources/Satis/packages-mirror Press Ctrl-C to quit.
[Sun Dec  8 14:48:09 2013] 127.0.0.1:56999 [200]: /
[Sun Dec  8 14:48:09 2013] 127.0.0.1:57000 [404]: /
favicon.ico
 - No such file or directory
And we can now browse our mirrored packages and even search for specific ones by pointing our web browser to http://localhost:4680:

MirrorWebpage

Let's Host It on Apache
If you have a running Apache at hand, creating a virtual host for Satis will be quite simple.

Listen 4680


 Options -Indexes FollowSymLinks AllowOverride all Order allow,deny Allow from all




 DocumentRoot "/path/to/your/packages-mirror" ServerName 127.0.0.1:4680 ServerAdmin admin@
example.com

 ErrorLog syslog:user



We just use a .conf file like this, put in Apache's conf.d folder, usually/etc/apache2/conf.d. It creates a virtual host on the 4680 port and points it to our folder. Of course you can use whatever port you want.

Updating Our Mirrors
Satis can not automatically update the mirrors unless we tell it. So the easiest way, on any UNIX like system, is to just add a cron job to your system. That would be very easy, and just a simple script to execute our update command.

#!/bin/bash php /full/path/to/satis/bin/satis build \
/full/path/to/
mirrored-packages.conf
 \
/full/path/to/packages-mirror
The drawback of this solution is that it is static. We have to manually update themirrored-packages.conf every time we add another package to our project'scomposer.json. If you are part of a team in a company with a big project and a continuous integration server, you can't rely on people remembering to add the packages on the server. They may not even have permissions to access the CI infrastructure.

Dynamically Updating Satis Configuration
It's time for a PHP TDD exercise. If you just want your code ready and running, check out the source code attached to this tutorial.

require_once __DIR__ . '/../../../../vendor/
autoload.php
';
 class SatisUpdaterTest extends PHPUnit_Framework_TestCase {
    function testBehavior() {
        $this->assertTrue(true);
    }
}
As usual we start with a degenerative test, just enough to make sure we have a working testing framework. You may notice that I have quite a strange looking require_once line, this is because I want to avoid having to reinstall PHPUnit and Mockery for each small project. So I have them in a vendor folder in my NetTuts' root. You should just install them with composer and drop the require_once line altogether.

class SatisUpdaterTest extends PHPUnit_Framework_TestCase {
    function testDefaultConfigFile() {
        $expected = '{
    "name": "NetTuts Composer Mirror",
    "homepage": "http://localhost:4680",

    "repositories": [
        { "type": "vcs", "url": "
https://github.com/SynetoNet/monolog
" },
        { "type": "composer", "url": "
https://packagist.org
" }
    ],

    "require": {
    },
    "require-dependencies": true
}';
        $actual = $this->parseComposerConf('');
        $this->assertEquals($expected, $actual);
    }
}
That looks about right. All the fields except "require" are static. We need to generate only the packages. The repositories are pointing to our private git clones and to packagist as needed. Managing those is more of a sysadmin job than a software developer's.

Of course this fails with:

PHP Fatal error:  Call to undefined method SatisUpdaterTest::parseComposerConf()
Fixing that is easy.

private function parseComposerConf($string) {
}
I just added an empty method with the required name, as private, to our test class. Cool, but now we have another error.

PHPUnit_Framework_ExpectationFailedException : Failed asserting that null matches expected '{ ... }'
So, null does not match our string containing all that default configuration.

private function parseComposerConf($string) {
    return '{
"name": "NetTuts Composer Mirror",
"homepage": "http://localhost:4680",

"repositories": [
    { "type": "vcs", "url": "
https://github.com/SynetoNet/monolog
" },
    { "type": "composer", "url": "
https://packagist.org
" }
],

"require": {
},
"require-dependencies": true
}';
}
OK, that works. All tests are passing.

PHPUnit 3.7.28 by Sebastian Bergmann. Time: 15 ms, Memory: 2.50Mb OK (1 test, 1 assertion)
But we introduced a horrible duplication. All that static text in two places, written character by character in two different places. Let's fix it:

class SatisUpdaterTest extends PHPUnit_Framework_TestCase {
    static $DEFAULT_CONFIG = '{
    "name": "NetTuts Composer Mirror",
    "homepage": "http://localhost:4680",

    "repositories": [
        { "type": "vcs", "url": "
https://github.com/SynetoNet/monolog
" },
        { "type": "composer", "url": "
https://packagist.org
" }
    ],

    "require": {
    },
    "require-dependencies": true
}';

    function testDefaultConfigFile() {
        $expected =  self::$DEFAULT_CONFIG;

        $actual = $this->parseComposerConf('');
        $this->assertEquals($expected, $actual);
    }

    private function parseComposerConf($string) {
        return self::$DEFAULT_CONFIG;
    }
}
Ahhh! That's better.

    function testEmptyRequiredPackagesInComposerJsonWillProduceDefaultConfiguration() {
    $expected = self::$DEFAULT_CONFIG;

    $actual = $this->parseComposerConf('{"require": {}}');
    $this->assertEquals($expected, $actual);
}
Well. That also passes. But it also highlights some duplication and useless assignment.

    function testDefaultConfigFile() {
    $actual = $this->parseComposerConf('');
    $this->assertEquals(self::$DEFAULT_CONFIG, $actual);
}
 function testEmptyRequiredPackagesInComposerJsonWillProduceDefaultConfiguration() {
    $actual = $this->parseComposerConf('{"require": {}}');
    $this->assertEquals(self::$DEFAULT_CONFIG, $actual);
}
We inlined the $expected variable. $actual could also be inlined, but I like it better this way. It keeps the focus on what is tested.

Now we have another problem. The next test I want to write would look like this:

function testARequiredPackageInComposerWillBeInSatisAlso() {
    $actual = $this->parseComposerConf(
    '{"require": {
        "Mockery/Mockery": ">=0.7.2"
    }}');
    $this->assertContains('"Mockery/Mockery": ">=0.7.2"', $actual);
}
But after writing the simple implementation, we will notice it requires json_decode()and json_encode(). And of course these functions reformat our string and matching strings will be difficult at best. We have to take a step back.

function testDefaultConfigFile() {
    $actual = $this->parseComposerConf('');
    $this->assertJsonStringEqualsJsonString($this->jsonRecode(self::$DEFAULT_CONFIG), $actual);
}
 function testEmptyRequiredPackagesInComposerJsonWillProduceDefaultConfiguration() {
    $actual = $this->parseComposerConf('{"require": {}}');
    $this->assertJsonStringEqualsJsonString($this->jsonRecode(self::$DEFAULT_CONFIG), $actual);
}
 private function parseComposerConf($jsonConfig) {
    return $this->jsonRecode(self::$DEFAULT_CONFIG);
}
 private function jsonRecode($json) {
    return json_encode(json_decode($json, true));
}
We changed our assertion method to compare JSON strings and we also recoded our $actual variable. ParseComposerConf() was also modified to use this method. You will see in a moment how it helps us. Our next test becomes more JSON specific.

function testARequiredPackageInComposerWillBeInSatisAlso() {
    $actual = $this->parseComposerConf(
        '{"require": {
            "Mockery/Mockery": ">=0.7.2"
        }}');
    $this->assertEquals('>=0.7.2', json_decode($actual, true)['require']['Mockery/Mockery']);
}
And making this test pass, along with the rest of the tests, is quite easy, again.

private function parseComposerConf($jsonConfig) {
    $addedConfig = json_decode($jsonConfig, true);
    $config = json_decode(self::$DEFAULT_CONFIG, true);
    if (isset($addedConfig['require'])) {
        $config['require'] = $addedConfig['require'];
    }
    return json_encode($config);
}
We take the input JSON string, decode it, and if it contains a "require" field we use that in our Satis configuration file instead. But we may want to mirror all versions of a package, not just the last one. So maybe we want to modify our test to check that the version is "*" in Satis, regardless of what exact version is in composer.json.

function testARequiredPackageInComposerWillBeInSatisAlso() {
    $actual = $this->parseComposerConf(
        '{"require": {
            "Mockery/Mockery": ">=0.7.2"
        }}');
    $this->assertEquals('*', json_decode($actual, true)['require']['Mockery/Mockery']);
}
That obviously fails with a cool message:

PHPUnit_Framework_ExpectationFailedException : Failed asserting that two strings are equal. Expected :* Actual   :>=0.7.2
Now, we need to actually edit our JSON before re-encoding it.

private function parseComposerConf($jsonConfig) {
    $addedConfig = json_decode($jsonConfig, true);
    $config = json_decode(self::$DEFAULT_CONFIG, true);
    $config = $this->addNewRequires($addedConfig, $config);
    return json_encode($config);
}
 private function toAllVersions($config) {
    foreach ($config['require'] as $package => $version) {
        $config['require'][$package] = '*';
    }
    return $config;
}
 private function addNewRequires($addedConfig, $config) {
    if (isset($addedConfig['require'])) {
        $config['require'] = $addedConfig['require'];
        $config = $this->toAllVersions($config);
    }
    return $config;
}
To make the test pass we have to iterate over each element of the required packages array and set their version to '*'. See method toAllVersion() for more details. And to speed up this tutorial a little bit, we also extracted some private methods in the same step. This way, parseComoserConf() becomes very descriptive and easy to understand. We could also inline $config into the arguments ofaddNewRequires(), but for aesthetic reasons I left it on two lines.

But what about "require-dev" in composer.json?

function testARquiredDevPackageInComposerWillBeInSatisAlso() {
    $actual = $this->parseComposerConf(
        '{"require-dev": {
            "Mockery/Mockery": ">=0.7.2",
            "phpunit/phpunit": "3.7.28"
        }}');
    $this->assertEquals('*', json_decode($actual, true)['require']['Mockery/Mockery']);
    $this->assertEquals('*', json_decode($actual, true)['require']['phpunit/phpunit']);
}
That obviously fails. We can make it pass with just copy/pasting our if condition inaddNewRequires():

private function addNewRequires($addedConfig, $config) {
    if (isset($addedConfig['require'])) {
        $config['require'] = $addedConfig['require'];
        $config = $this->toAllVersions($config);
    }
    if (isset($addedConfig['require-dev'])) {
        $config['require'] = $addedConfig['require-dev'];
        $config = $this->toAllVersions($config);
    }
    return $config;
}
Yep, that makes it pass, but those duplicated if statements are nasty looking. Let's deal with them.

private function addNewRequires($addedConfig, $config) {
    $config = $this->addRequire($addedConfig, 'require', $config);
    $config = $this->addRequire($addedConfig, 'require-dev', $config);
    return $config;
}
 private function addRequire($addedConfig, $string, $config) {
    if (isset($addedConfig[$string])) {
        $config['require'] = $addedConfig[$string];
        $config = $this->toAllVersions($config);
    }
    return $config;
}
We can be happy again, tests are green and we refactored our code. I think only one test is left to be written. What if we have both "require" and "require-dev" sections in composer.json?

function testItCanParseComposerJsonWithBothSections() {
    $actual = $this->parseComposerConf(
        '{"require": {
            "Mockery/Mockery": ">=0.7.2"
            },
        "require-dev": {
            "phpunit/phpunit": "3.7.28"
        }}');
    $this->assertEquals('*', json_decode($actual, true)['require']['Mockery/Mockery']);
    $this->assertEquals('*', json_decode($actual, true)['require']['phpunit/phpunit']);
}
That fails because the packages set by "require-dev" will overwrite those of "require" and we will have an error:

Undefined index: Mockery/Mockery
Just add a plus sign to merge the arrays, and we are done.

private function addRequire($addedConfig, $string, $config) {
    if (isset($addedConfig[$string])) {
        $config['require'] += $addedConfig[$string];
        $config = $this->toAllVersions($config);
    }
    return $config;
}
Tests are passing. Our logic is finished. All we left to do is to extract the methods into their own file and class. The final version of the tests and the SatisUpdater class can be found in the attached source code.

We can now modify our cron script to load our parser and run it on ourcomposer.json. This will be specific to your projects' particular folders. Here is an example you can adapt to your system.

#!/usr/local/bin/php


Configuration.php
';

$outputDir = '/path/to/your/packages-mirror';
$composerJsonFile = '/path/to/your/projects/
composer.json
';
$satisConf = '/path/to/your/
mirrored-packages.conf
';

$satisUpdater = new SatisUpdater();
$conf = $satisUpdater->parseComposerConf(file_get_contents($composerJsonFile)); file_put_contents($satisConf, $conf);
 system(sprintf('/path/to/satis/bin/satis build %s %s', $satisConf, $outputDir), $retval); exit($retval);
Making Your Project Use the Mirror
We talked about a lot of things in this article, but we did not mention how we will instruct our project to use the mirror instead of the Internet. You know, the default ispackagist.org? Unless we do something like this:

"repositories": [
     {
         "type": "composer",
         "url": "http://your-mirror-server:4680"
     }
 ],
That will make your mirror the first choise for composer. But adding just that into thecomposer.json of your project will not disable access to packagist.org. If a package can not be found on the local mirror, it will be downloaded from the Internet. If you wish to block this feature, you may also want to add the following line to the repositories section above:

"packagist": false
Final Thoughts
That's it. Local mirror, with automatic adapting and updating packages. Your colleagues will never have to wait a long time while they or the CI server installs all the requirements of your projects. Have fun.

文章出处: Patkos Csaba
文章地址: http://code.tutsplus.com/tutorials/setting-up-a-local-mirror-for-composer-packages-with-satis--net-36726
本文地址: http://www.webrube.com/composer-php-web_rube/4600
本文由 噜吧 整理，转载请保留以上信息; 如有侵犯您的版权, 请联系 webrube.com@gmail.com 。
