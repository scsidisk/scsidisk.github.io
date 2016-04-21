终极 Web 应用性能和压力测试工具
============================

转自：https://blog.eood.cn/web-performance-testing-gor

![](https://blog.eood.cn/wp-content/uploads/2015/06/gor.png "gor")

### 常见的 Web 应用的压力测试工具

Web 应用压力测试工具有很多，比如 Apache
ab，[node-ab](https://github.com/doubaokun/node-ab)，Apache JMeter,
LoadRunner, httperf。但是这些工具都没能解决一个问题：

**如何正确模拟生产环境的流量**

如今 Web 应用的架构变得非常复杂，内部包含复杂的各种负载均衡、 服务和 RPC
调用关系，简单的发送 GET 请求到某些 URL 或者 API
接口完全无法模拟真实的流量。假如回放 HTTP 日志，操作又异常麻烦。Tcpcopy
虽然能够复制实时流量，但是操作也很复杂。之前的
[亚马逊云平台的迁移](http://blog.eood.cn/aws_cloud_migration) 就用到了
Gor 这个工具。

### Gor 是 Web 应用压力测试的完美方案

我一直在找一个简单又方便的解决方案，直到找到了 Gor 。Gor 是用 Golang
写的一个 HTTP 实时流量复制工具。只需要在 LB 或者 Varnish
入口服务器上执行一个进程，就可以把生产环境的流量复制到任何地方，比如
Staging 环境、Dev 环境。完美解决了 HTTP 层实时流量复制和压力测试的问题。

### Gor 的功能

Gor
支持流量的放大和缩小、频率限制，这样不需要搭建和生产环境一致的服务器集群也可以正确测试。Gor
还支持根据正则表达式过滤流量，这意味着可以单独测试某个 API
服务。还可以修改 HTTP 请求头，比如替换 User-Agent, 或者增加某些 HTTP
Header 。

Gor 还可以把请求记录到文件，以备回放和分析。Gor 支持和 ElasticSearch
集成，将流量存入 ES 进行实时分析。

### Gor 的常用命令

**简单的 HTTP 流量复制：**

    gor –input-raw :80 –output-http “http://staging.com”

**HTTP 流量复制频率控制：**

    gor –input-tcp :28020 –output-http “http://staging.com|10″

**HTTP 流量复制缩小：**

    gor –input-raw :80 –output-tcp “replay.local:28020|10%”

**HTTP 流量记录到本地文件：**

    gor –input-raw :80 –output-file requests.gor

**HTTP 流量回放和压测：**

    gor –input-file “requests.gor|200%” –output-http “staging.com”

**HTTP 流量过滤复制：**

    gor –input-raw :8080 –output-http staging.com –output-http-url-regexp \^www.

### 最后

这个 Golang 写的 Gor
是开源的，意味着可以方便的集成到自己的[架构](http://blog.eood.cn/category/architecture)中，可以用在压力测试平台、实时流量分析、应用层防火墙等等方面。

### 有用的链接

- https://github.com/buger/gor
- https://github.com/doubaokun/node-ab
- https://github.com/session-replay-tools/tcpcopy
- https://github.com/httperf/httperf
- https://github.com/buger/gor/blob/master/ELASTICSEARCH.md


