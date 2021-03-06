---
layout: post
title: "调优Apache, PHP"
date: 2010-12-28 14:57
author: scsidisk
categories: PHP
tags: Apache, PHP, 调优
---

Apache
是一种高度可配置的软件。它具有大量特性，但每一种都代价高昂。从某种程度上来说，调优
Apache来说就是以恰当的方式分配资源，还涉及到将配置简化为仅包含必要内容。

配置 MPM

Apache是模块化的，因为可以轻松添加和移除特性。在 Apache
的核心，多处理模块（Multi-Processing Module，MPM）提供了这种模块化功能性
—— 管理网络连接、调度请求。MPM使您能够使用线程，甚至能够将
Apache迁移到另外一个操作系统。

每次只能有一个 MPM 是活动的，必须使用 --with-mpm=(worker|prefork|event)
静态编译。

每个请求使用一个进程的传统模型称为 prefork。 较新的线程化模型称为
worker，它使用多个进程，每个进程又有多个线程，这样就能以较低的开销获得更好的性能。最新的
event MPM
是一种实验性的模型，为不同的任务使用单独的线程池。要确定当前使用的是哪种
MPM，可执行 httpd -l。

如:

[root@localhost \~]\# httpd -l

Compiled in modules:

core.c

prefork.c

http\_core.c

mod\_so.c

选择使用何种 MPM 取决于许多因素。在 event MPM
脱离实验状态之前，不应考虑这种模型，而是在使用线程和不使用线程之间作出选择。表面上看来，如果所有底层模块（包括
PHP 使用的所有库）都是线程安全的，线程要优于分叉（forking）。而 Prefork
是较为安全的选择；如果选择了worker，
则应该谨慎测试。性能收益还取决于您的发布版所附带的库及硬件。

无论选择了哪种 MPM，都必须恰当地配置它。一般而言，配置 MPM包括告知
Apache怎样去控制有多少 worker正在运行，它们是线程还是进程。prefork
MPM的重要配置选项如清单 1 所示。

清单 1. prefork MPM 的配置

StartServers 50

MinSpareServers 15

MaxSpareServers 30

MaxClients 225

MaxRequestsPerChild 4000

prefork
模型会为每个请求创建一个新进程。多余的进程保持空闲，以处理传入的请求，这缩短了启动延迟。只要
Web 服务器出现，预先完成的配置就会立即启动 50 个进程，并尽力保持 10 到
20 个空闲服务器运行。进程数的硬性限制由 MaxClients
指定。尽管一个进程能够处理许多相继的请求，Apache 还是会取消连接数超过
4,000 以后的进程，这降低了内存泄漏的风险。

配置线程化 MPM 与之类似，不同之处只是必须确定使用多少线程和进程。Apache
文档解释了所有必要的参数和计算。

要经过几次尝试和出错之后才能选好要使用的值。最重要的值是
MaxClients。目标在于允许足够多的 workder
进程或线程运行，同时又不会导致服务器进行过度的交换。如果传入的请求超出处理能力，那么至少满足此值的那些请求会得到服务，其他请求被阻塞。

如果 MaxClients 过高，那么所有客户机都将体验到糟糕的服务，因为 Web
服务器会试图换出一个进程，以使另一个进程能够运行。而设得过低意味着可能会不必要地拒绝服务。查看高负载下运行的进程数量和所有
Apache 进程所导致的内存占用情况对设置这个值很有帮助。如果 MaxClients
的值超过 256，必须将 ServerLimit 也设为同样的数值，请仔细阅读 MPM
的文档，了解相关信息。

根据服务器的角色调优要启动和保持空闲的服务器数量。如果服务器仅运行
Apache，那么可以使用适中的值，如 清单 1
所示，因为这样就能充分利用机器。如果系统中还有其他数据库或服务器，那么就应该限制运行中的空闲服务器的数量。

有效地使用选项和重写

Apache 处理的每个请求都要履行一套复杂的规则，这些规则指明了 Web
服务器必须遵循的约束或特殊指令。对文件夹的访问可能按 IP
地址约束为某个特定文件夹，也可配置用户名和密码。这些选项还包含处理特定文件，例如，如果提供了一个目录列表，该如何处理的文件，或输出结果是否应压缩。

这些配置以 httpd.conf 中容器的形式出现，例如
\<Directory\>，以便指定所用配置引用的是磁盘上的一个位置；再如
\<Location\>，表示引用是 URL 中的路径。清单 2 展示了一个实际的 Directory
容器。

清单 2. 为根目录应用的一个 Directory 容器

\<Directory /\>

AllowOverride None

Options FollowSymLinks

\</Directory\>

在清单 2 中，位于一对 Directory 和 /Directory
标记之间的配置应用于给定目录和该目录下的一切内容 ——
在本例中，这个给定目录是根目录。此处，AllowOverride
标记指出，用户不允许重写任何选项（稍后将进一步介绍）。FollowSymLinks
选项被启用，它允许 Apache
查看之前的符号连接来为请求提供服务，即便文件位于包含 Web
文件的目录之外。这就意味着，如果 Web 目录中的一个文件是 /etc/passwd
的符号连接，Web 服务器将在请求时顺利为该文件提供服务。如果使用了
-FollowSymLinks，该特性就会被禁用，同样的请求将致使为客户机返回错误。

最后这个场景正是导致两方面关注的原因所在。第一个方面与性能有关。如果禁用了
FollowSymLinks，Apache
就必须检查使用该文件名的所有组件（目录和文件本身），以确保它们不是符号连接。这会带来额外的开销（磁盘操作）。另外一个称为
FollowSymLinksIfOwnerMatch
的选项会在文件所有者与连接所有者相同时使用符号连接。为获得最佳性能，请使用
清单 2 中的选项。

至此，有安全意识的读者应该有了警惕的感觉。安全性永远是功能性与风险之间的权衡。在我们的例子中，功能性是速度，而风险是允许对系统上的文件进行未经授权的访问。缓解风险的措施之一是
LAMP
应用服务器通常专注于一种具体功能，用户无法创建危险的符号连接。如果有必要启用符号连接，那么可以将其约束在文件系统的特定区域，如清单
3 所示。

清单 3. 将 FollowSymLinks 约束为一个用户的目录

\<Directory /\>

Options FollowSymLinks

\</Directory\>

\<Directory /home/\*/public\_html\>

Options -FollowSymLinks

\</Directory\>

在清单 3 中，一个用户的主目录中的任何 public\_html
目录及其所有子目录都移除了 FollowSymLinks 选项。

如您所见，通过主服务器配置，可为每个目录单独配置选项。用户可以自行重写这种服务器配置（如果管理员通过
AllowOverrides 语句允许了这种操作），只需将一个 .htaccess
文件放入目录即可。该文件包含额外的服务器指令，每次请求包含 .htaccess
文件的目录时将加载并应用这些指令。尽管之前探讨过系统没有用户的问题，但许多
LAMP 应用程序都利用这种功能性来控制访问、实现 URL
重写，因此有必要理解其工作原理。

即便 AllowOverrides 语句能阻止用户去做您不希望他们做的事，Apache
也必须检查 .htaccess
文件，看看是否有要完成的工作。父目录可以指定由来自子目录的请求处理的指令，这也就表示，Apache
必须搜索所请求文件的目录树的所有组件。可想而知，这会使每次请求都导致大量磁盘操作。

最简单的解决方案是不允许重写，这能消除 Apache 检查 .htaccess
的需求。之后的任何特殊配置都将直接放在 httpd.conf 中。清单 4
显示为对一个用户的项目目录进行密码检查向 httpd.conf
增加的代码，而不是将其放入一个 .htaccess 文件并依赖于 AllowOverrides。

清单 4. 将 .htaccess 配置移入 httpd.conf

\<Directory /home/user/public\_html/project/\>

AuthUserFile /home/user/.htpasswd

AuthName "uber secret project"

AuthType basic

Require valid-user

\</Directory\>

如果配置转移到 httpd.conf 中，且 AllowOverrides
被禁用，磁盘的使用就能减少。一个用户的项目可能不会吸引许多人来点击，但设想一下，将这项技术应用于一个忙碌的站点时会有多么强大。

有时不可能彻底消除 .htaccess 文件的使用。例如，在清单 5
中，一个选项被约束到文件系统的特定部分，重写也可以是有作用域的。

清单 5. 限定 .htaccess 检查的作用域

\<Directory /\>

AllowOverrides None

\</Directory\>

\<Directory /home/\*/public\_html\>

AllowOverrides AuthConfig

\</Directory\>

实现清单 5 之后，Apache 会在父目录中查找 .htaccess 文件，但会在
public\_html
目录处停止，因为文件系统的其余部分禁用了此功能。例如，如果请求的是一个映射到
/home/user/public\_html/project/notes.html 的文件，那么仅有 public\_html
和 project 目录被搜索。

关于每目录单独配置的最后一个提示就是：要按顺序依次进行。任何介绍 Apache
调优的的文章都会告诉您，应通过 HostnameLookups off 指令禁用 DNS
查找，因为试图反向解析连接到您的服务器的所有 IP
地址无疑是浪费资源。然而，基于主机名的任何约束都会迫使 Web
服务器对客户机的 IP
地址执行反向查找，对其结果进行正向查找，以验证该名称的真实性。因此，避免使用基于客户主机名的访问控制，在必须使用时限定其作用域，这些都是明智的做法。

持久连接

一个客户机连接到 Web 服务器时，允许客户机通过同一个 TCP
连接发出多个请求，这减少了与多个连接相关的延迟。在一个 Web
页面引用了多幅图片时，这就很有用：客户机可以通过一个连接先请求页面，再请求所有图片。其缺点在于服务器上的
worker 进程必须等待客户机要关闭的会话，之后才能转到下一个请求。

Apache 使您能够配置如何处理持久连接（称为 keepalives）。httpd.conf
全局级的 KeepAlive 5 允许服务器在连接强制关闭之前处理一个连接上的 5
个请求。将此值设置为 0 将禁用持久连接。同样位于全局级上的
KeepAliveTimeout 确定在会话关闭之前，Apache 将等待另外一个连接多久。

持久连接的处理并非 “一刀切” 式的配置。对于某些 Web 站点，禁用 keepalives
更合适（KeepAlive
0）；而对于其他一些站点，启用它会带来巨大的收益。惟一的解决之道就是尝试使用这两种配置，自己观察哪种更合适。但若启用了
keepalives，使用较小的超时时间较为明智，例如 2，即 KeepAliveTimeout
2。这能确保希望发出另外一个请求的客户机有充足的时间，还能确保 worker
进程不会一直空闲，等待可能永远不会出现的下一个请求。

压缩

Web 服务器能够在将输出发回给客户机之前压缩它。这将使通过 Internet
发送的页面更小，代价是 Web 服务器上的 CPU 周期。对于那些负担得起 CPU
开销的服务器来说，这是提高页面下载速度的好办法 ——
页面压缩后大小变为原来的三分之一这种事情并不罕见。

图片通常已经是压缩过的，因此压缩应仅限于文本输出。Apache 通过
mod\_deflate 提供压缩。尽管 mod\_deflate
可轻松启用，但它涉及到太多的复杂性，很多手册都解释了这些复杂的内容。本文不会介绍压缩的配置，但提供了相应文档的链接（参见
参考资料 部分）。

调优 PHP

PHP 是运行应用程序代码的引擎。应该仅安装计划使用的那些模块，并配置您的
Web 服务器，使之仅为脚本文件（通常是以 .php 结尾的那些文件）使用
PHP，而非所有静态文件。

操作码缓存

请求一个 PHP 脚本时，PHP 会读取该脚本，并将其编译为 Zend
操作码，这是要执行的代码的一种二进制表示形式。随后，此操作码由 PHP
执行并丢弃。操作码缓存将保存这个编译后的操作码，并在下一次调用该页面时重用它。这会节省很多时间。有多种缓存可用，我比较常用的是
eAccelerator。

要安装 eAccelerator，您的计算机上需要有 PHP 开发库。由于不同的 Linux
发布版存放文件的位置不同，所以最好直接从 eAccelerator 的 Web
站点获得安装说明（参见 参考资料
部分获得链接）。您的发布版也有可能已经包含了一个操作码缓存，只需安装即可。

无论如何在系统上安装
eAccelerator，都有一些配置选项需要注意。配置文件通常是
/etc/php.d/eaccelerator.ini。eaccelerator.shm\_size
定义共享高速缓存的大小，编译后的脚本就存储在这里。该值的单位是兆字节（MB）。根据您的应用程序确定恰当的大小。eAccelerator
提供了一个脚本来显示缓存的状态，其中包含内存占用，64MB
是个不错的选择（eaccelerator.shm\_size="64"）。如果您选择的值未被接受，那么必须修改内核的最大共享内存的大小。向
/etc/sysctl.conf 添加 kernel.shmmax=67108864，运行 sysctl -p
来使设置生效。kernel.shmmax 值的单位是字节。

如果共享内存的分配超出极限，eAccelerator
必须将旧脚本从内存中清除。默认情况下，这是被禁用的；eaccelerator.shm\_ttl
= "60" 指定：当 eAccelerator 用完共享内存时，60
秒内未被访问的所有脚本都将被清除。

另一种流行的 eAccelerator 替代工具是 Alternative PHP Cache（APC）。Zend
的厂商也提供了一种商业操作码缓存，包括一个进一步提高效率的优化器。

php.ini

PHP 的配置是在 php.ini 中完成的。四个重要的设置控制 PHP
可使用多少系统资源，如表 1 所列。

表 1. php.ini 中与资源相关的设置

设置 描述 建议值

max\_execution\_time 一个脚本可使用多少 CPU 秒 30

max\_input\_time 一个脚本等待输入数据的时间有多长（秒） 60

memory\_limit 在被取消之前，一个脚本可使用多少内存（字节） 32M

output\_buffering 数据发送给客户机之前，有多少数据（字节）需要缓存 4096

具体数字主要取决于您的应用程序。如果要从用户处接收大文件，那么
max\_input\_time 可能必须增加，可以在 php.ini
中修改，也可以通过代码重写它。与之类似，CPU
或内存占用较多的程序也可能需要更大的设置值。目标就是缓解超标程序的影响，因此不建议全局禁用这些设置。关于
max\_execution\_time，还有一点需要注意：它表示进程的 CPU
时间，而不是绝对时间。因此一个进行大量 I/O
和少量计算的程序的运行时间可能远远超过 max\_execution\_time。这也是
max\_input\_time 可以大于 max\_execution\_time 的原因所在。

PHP
可执行的日志记录数是可配置的。在生产环境中，禁用除最重要的日志以外的一切日志记录能够减少磁盘写操作。如果需要使用日志来排除问题，那么可以按需启用日志记录。error\_reporting
= E\_COMPILE\_ERROR|E\_ERROR|E\_CORE\_ERROR
将启用足够的日志记录，使您发现问题，同时从脚本中消除大量无用的内容。
