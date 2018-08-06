十个免费的Web压力测试工具
=========================

2010年7月13日 [陈皓](http://coolshell.cn/articles/author/haoel "由陈皓发布")

两天，jnj在本站发布了《[如何在低速率网络中测试
Web
应用](http://coolshell.cn/articles/2574.html)》，那是测试网络不好的情况。而下面是十个免费的可以用来进行Web的负载/压力测试的工具，这样，你就可以知道你的服务器以及你的WEB应用能够顶得住多少的并发量，以及你的网站的性能。我相信，北京奥组委的订票网站的开发团队并不知道有这样的测试工具。

**[Grinder](http://grinder.sourceforge.net/)** –
 Grinder是一个开源的JVM负载测试框架，它通过很多负载注射器来为分布式测试提供了便利。
支持用于执行测试脚本的Jython脚本引擎HTTP测试可通过HTTP代理进行管理。根据项目网站的说法，Grinder的
主要目标用户是“理解他们所测代码的人——Grinder不仅仅是带有一组相关响应时间的‘黑盒’测试。由于测试过程可以进行编码——而不是简单地脚本
化，所以程序员能测试应用中内部的各个层次，而不仅仅是通过用户界面测试响应时间。

**[Pylot](http://www.pylot.org/)** -Pylot是一款开源的测试web
service性能和扩展性的工具，它运行HTTP
负载测试，这对容量计划，确定基准点，分析以及系统调优都很有用处。Pylot产生并发负载（HTTP
Requests），检验服务器响应，以及产生带有metrics的报表。通过GUI或者shell/console来执行和监视test
suites。

[**Web Capacity Analysis Tool
(WCAT)**](http://www.iis.net/community/default.aspx?tabid=34&i=1466&g=6)
– 这是一种轻量级负载生成实用工具，不仅能够重现对 Web
服务器（或负载平衡服务器场）的脚本 HTTP
请求，同时还可以收集性能统计数据供日后分析之用。WCAT
是多线程应用程序，并且支持从单个源控制多个负载测试客户端，因此您可以模拟数千个并发用户。该实用工具利用您的旧机器作为测试客户端，其中每个测试客户端又可以产生多个虚拟客户端（最大数量取决于客户端机器的网络适配器和其他硬件）。您可以选择使用
HTTP 1.0 还是 HTTP 1.1 请求，以及是否使用
SSL。并且，如果测试方案需要，您还可以使用脚本执行的基本或 NTLM
身份验证来访问站点的受限部分。（如果您的站点使用
cookie、表单或基于会话的身份验证，那您可以创建正确的 GET 或 POST
请求来对测试用户进行身份验证。）WCAT 还可管理您站点可能设置的任何
cookie，所以配置文件和会话信息将永久保存。



**[fwptt](http://fwptt.sourceforge.net/index.html)** – fwptt
也是一个用来进行WEB应用负载测试的工具。它可以记录一般的请求，也可以记录Ajax请求。它可以用来测试 asp.net，
jsp， php 或是其它的Web应用。

**[JCrawler](http://jcrawler.sourceforge.net/)** –
JCrawler是一个开源([CPL](http://www.opensource.org/licenses/cpl.php))
的WEB应用压力测试工具。通过其名字，你就可以知道这是一个用Java写的像网页爬虫一样的工具。只要你给其几个URL，它就可以开始爬过去了，它用一种特殊的方式来产生你WEB应用的负载。这个工具可以用来测试搜索引擎对你站点产生的负载。当然，其还有另一功能，你可以建立你的网站地图和再点击一下，将自动提交Sitemap给前5名的搜索引擎！

**[Apache JMeter](http://jakarta.apache.org/jmeter/)** – Apache
JMeter是一个专门为运行和服务器装载测试而设计的、100％的纯Java桌面运行程序。原先它是为Web/HTTP测试而设计的，但是它已经扩展以支持各种各样的测试模块。它和用于HTTP和SQL数据库（使用JDBC）的模块一起运送。它可以用来测试静止资料库或者活动资料库中的服务器的运行情况，可以用来模拟对服务器或者网络系统加以重负荷以测试它的抵抗力，或者用来分析不同负荷类型下的所有运行情况。它也提供了一个可替换的界面用来定制数据显示，测试同步及测试的创建和执行。

**[Siege](http://www.joedog.org/index/siege-home)**
-Siege（英文意思是围攻）是一个压力测试和评测工具，设计用于WEB开发这评估应用在压力下的承受能力：可以根据配置对一个WEB站点进行多用户的并发访问，记录每个用户所有请求过程的相应时间，并在一定数量的并发访问下重复进行。 Siege
支持基本的认证，cookies， HTTP 和 HTTPS 协议。

**[http\_load](http://www.acme.com/software/http_load/)** – http\_load
以并行复用的方式运行，用以测试web服务器的吞吐量与负载。但是它不同于大多数压力测试工具，它可以以一个单一的进程运行，一般不会把客户机搞死。可以可以测试HTTPS类的网站请求。

**[Web Polygraph](http://www.web-polygraph.org/)** – Web
Polygraph这个软件也是一个用于测试WEB性能的工具，这个工具是很多公司的标准测试工具，包括微软在分析其软件性能的时候，也是使用这个工具做为基准工具的。很多招聘测试员的广告中都注明需要熟练掌握这个测试工具。

**[OpenSTA](http://opensta.org/)** –
OpenSTA是一个免费的、开放源代码的web性能测试工具，能录制功能非常强大的脚本过程，执行性能测试。例如虚拟多个不同的用户同时登陆被测试网站。其还能对录制的测试脚本进行,按指定的语法进行编辑。在录制完测试脚本后，可以对测试脚本进行编辑，以便进行特定的性能指标分析。其较为丰富的图形化测试结果大大提高了测试报告的可阅读性。OpenSTA 基于CORBA 的结构体系，它通过虚拟一个proxy，使用其专用的脚本控制语言，记录通过proxy 的一切HTTP/S
traffic。通过分析OpenSTA的性能指标收集器收集的各项性能指标，以及HTTP 数据，对系统的性能进行分析。

欢迎您留下你认为不错的WEB应用性能测试的工具。

**（转载本站文章请注明作者和出处 [酷 壳 –
CoolShell.cn](http://coolshell.cn/) ，请勿用于任何商业用途）**
