---
layout: post
title: "微信支付：如何打造移动支付时代的高可用收银系统？"
date: 2017-04-10
author: 郭润增等
category: MacOSX
tags: [Retail,Pay]
---

编者按：本文来自微信公众号“InfoQ”（ID：infoqchina），作者为微信支付商户运营开发团队；36氪经授权发布。

移动支付时代，越来越多的人习惯于不带现金出门，许多支付场景只需要掏出手机就能完成。正因为如此，收银系统的可用性问题也越来越重要。如何打造移动支付时代的高可用收银系统？这是微信支付团队的经验，仅供参考。

### 为什么强调收银系统的可用性？

随着移动支付高速发展，用户已养成出门消费不带钱包的习惯， 频繁的日常消费对商户收银系统高可用提出了极高的要求，收银系统一点小小的故障如“付不了钱、重复支付、付款超时”等都会給用户和商户带来诸多的不适和不利，引发用户愤怒、投诉、纠纷，最终导致商户的用户流失。所以对于商户来说如何打造高可用的收银系统就变得十分的重要。

如何打造高可用收银系统？看完本文，相信您将有所启发。

### 高可用收银系统设计方案

通过对市面上的收银系统进行分析研究，发现普遍存在以下风险：

1. 服务时延不稳定：
    - 跨城调用、DNS配置不当，导致网络不稳定；
2. 系统可用性考虑不周：
    - 多个支付渠道（支付宝、微信等）部署在一起，相互影响；
    - 业务逻辑服务和数据服务部署在一起，相互影响；
    - 无异地容灾、自动切换能力；
3. 数据容灾恢复不及时：
    - DB单点、主备切换依赖人工、故障恢复时间（TTR）不可控；

为了帮助商户提升服务质量，尽可能降低以上风险，微信支付团队提出一套高可用收银系统的设计方案，其系统架构图如下所示：

![](/static/images/2017/04/g3udarmrfi5mzr18!1200.jpeg)

接下来从三个层面分别阐述：

**1. 降低服务时延：**

收银系统线下门店遍布全国、网络复杂（包含电信、联通、铁通、移动等），对系统时延提出更高挑战。

针对这个问题，一些云服务商支持“BGP网络访问跨地域实时切换”的能力，通过冗余网络出口部署，实现跨区域网络间灵活切换调度，为网络出口灾备提供了保障。

另外腾讯云联合微信支付推出支付加速方案，部署在腾讯云上的服务可以直接将发往微信支付的公网请求解析为内网访问，将延时率减少30%，提升用户支付体验。

同时，微信支付官方还提供了api和api2两个API域名，供服务商系统自行探测服务质量，优先选择速度更快的域名进行访问。

注：双域名探测择优有如下注意点：

- 并发探测，谁先回来谁先被采用，从而提升效率；
- 建立探测重试机制、控制探测频率，减少不必要的探测；
- 建议的探测时机：系统开启时发起探测，或请求超时发起探测；

**2.云助力，低成本提升可用性：**

文章开头提到，在移动支付时代，用户对收银系统的可用性有更高的要求，这就迫使服务商做系统设计需要考虑更多因素。

由于这些因素实现成本比较高，纯粹自己实现的话不太现实，所以这里笔者将结合比较熟悉的腾讯云提供的能力来进行阐述，建议身处云时代的服务商多了解这些能力，低成本解决高可用问题。

因素一、多地部署、多点接入：

利用腾讯云在全球20多个数据中心的基础设施，很容易实现多地部署和多点接入，在架构层的高可用设计可以最大限度容忍单个地域网络运营商故障和网络抖动带来的不稳定因素，并为全球各地的业务伙伴提供最优质的接入条件。

当网络出现故障时，腾讯云全球内网互联互通，及时调度业务流量至其他区域可以保证用户体验不受影响。

因素二、防DDoS攻击：

DDoS攻击将真正的用户挡在门外，现在云服务商也会提供防御此类攻击的服务。比如腾讯云大禹BGP高防系统提供800G防护带宽和21道BGP线路，可以动态调度网络流量，帮助用户有效抵御DDoS攻击。

因素三、负载均衡、故障屏蔽：

为了提升系统的稳定性和容灾能力，业界比较成熟的解决方案是基于“无状态的应用层服务设计”，做到能够“实时监控服务器节点可用状态、自动转移失败任务到其他可用节点、将集中请求分摊到集群各个机器节点的能力”。

身处云时代服务商可以借助腾讯云的负载均衡（CLB）能力来低成本解决这个问题。腾讯云的负载均衡具备健康检查能力，可允许用户自定义健康检查频率，以确保后端云服务器在出现故障时第一时间感知到并且及时切走业务流量，保证前端应用的高可用和无感知。

CLB 单集群4台物理服务器组成，最大并发连接数超过1.2亿，可处理峰值40Gbps的流量，每秒处理包量为600万。只有一台实例可用的极端情况下，仍可支撑3000万以上的并发连接，确保后端正常提供服务，高扩展和低成本的优势最大限度节省IT成本。

因素四、过载保护：

移动支付目前处于高速增长期，各种营销活动会带来业务高峰。

一方面需要及时扩容，预留冗余服务能力；另一方面当实际业务流量远超过系统的最大正常服务水平时进行自我保护，快速拒绝掉部分请求，保证正常的服务水平，而不是被拖垮影响全部服务；

建议采用云服务商提供的消息队列，通过云上的分布式消息队列CMQ提供可靠的异步通信，有效提升系统吞吐量，确保消息的可靠传递，减少后端系统压力，防止系统雪崩。

另外腾讯云服务器具备弹性伸缩（Auto Scaling）能力，只需配置简单的伸缩规则，集群即可在高负载时自动扩容缩容，确保业务平稳度过高峰。按量计费能力可以最大限度节省IT成本。

**3.“跳单”，实现数据层秒级自动容灾切换能力：**

收银系统的数据分成两类，一类是订单信息（主要包括订单表和退款表，特点是数据量大且多读多写）；另一类是基础信息（主要包含门店、设备、商户等信息，特点是数据量少且多读少写）。这里介绍的数据库高效容灾实践是基于订单信息DＢ，以MySQL为例。

MySQL容灾策略普遍依赖“半同步，主备切换”，通过自动或者人工切换（业务恢复时间在1分钟到几十分钟之间）。这对于交易量稍大的场景来讲，故障恢复时间还是太长。如何在更短的时间内达到恢复业务，我们设计了“跳单”的数据层容灾解决方案。

核心思路：

在数据访问层封装一个“跳单”组件“自动避开有故障的存储”，让订单数据数据可以随意落到各个容器。

下面详细介绍“跳单”的整体流程：

![](/static/images/2017/04/0otz97wa1fxtw8gm!1200.jpeg)

为了实现跳单逻辑，我们先将数据库水平划分为若干组，每组DB一主两备、读写分离，主DB用来写，从DB用于读，主从同步由MySQL半同步机制来保证。

使用订单号保存分组标记，如原先单号为201609121215432322199，可以在最后一位加分组标识，如组2，则变成2016091212154323221992

在这样的前提下：

a）创建订单请求：

收银终端的一个创建订单请求过来，先调用DB选择器随机选择一组DB，然后查询计数器，看该组DB失败次数是否超出阈值，超限则跳过重选，否则通过探测器发送一条update语句，探测DB是否可用。如失败则需重选DB，成功则把分组标记写到单号，把订单插入改组DB。

b）更新或查询请求：

直接解析单号的分组标记，然后操作对应DB。“跳单”保证新交易是正常的，优先把支付做成。某组DB发生故障时，订单查询和撤销等操作需等主备切换恢复才能进行。

这里的注意事项：

- 计数器需要设置周期，比如一分钟，以便设备故障恢复自动启用。
- 调用MYSQL的时候需要设置超时时间，比如1秒，避免某一组DB故障，拖死上层服务。
- 探测使用update语句，这是因为DB如果死机恢复了之后有可能是只读状态，如果发送select来探测就无法保证DB是可写的。

### 做了“跳单“后的日常演练

为检测系统是否真正高可用，需要做定期演练，以下是我们的日常演练计划：

- 每周做一次单组DB故障的常规演练。
- 每季度做一次多组DB故障演练。

从下图演练时的监控可见，当某组DB故障时请求会掉底发生跳单，但整体曲线平滑，业务运行正常，无影响。

![](/static/images/2017/04/62demki7tbyaf5s9!1200.jpeg)

### 做了“跳单”后的扩容和缩容

扩容步骤：

- 部署新的订单DB，给它分配DB编号；
- 将新库信息配置到DB选择组件；
- 新库接入业务流量；
- 观察监控有无异常；

缩容步骤：

- 将撤掉DB的库编号按收缩之后剩余DB数取模：例如原来有5组DB，收缩到3组，计划撤掉库5，则先把5库模3得到2。
- 把数据迁移到库2，修改配置，关闭库5流量。新的订单不会再进入库5，而历史查询则通过取模访问库2即可。
- 监控无异常之后正式撤掉库5。

### 做了“跳单”后的商户维度查询

多组DB容灾方案有一个通用难题就是“商户维度列表查询效率问题”。订单分散在不同的DB，若查询量小则可直接采用全库扫描，通过上层并发调用来解决效率问题。如果变成高频操作，则需考虑额外搭建一套数据库，以商户纬度进行数据存储，这两套数据库之间的数据同步采用可靠消息队列来进行同步。具体推荐了解下腾讯云上面的PGXZ和MQ组件。

虽然因为“跳单”而带来了列表查询的效率问题，但是对收银系统来说，核心设计理念还是“尽可能把支付做成”！不要因为列表查询问题而影响到核心支付的可用性。

### 收银系统安全性考虑

系统安全性也是衡量一个收银系统可用性的关键指标，通过调研发现线下收银系统有可能存在以下安全风险：

- 收银终端软件被非法安装；
- 整台POS机被盗；
- 中间人攻击；
- 正常交易订单被非法退款；

为了应对上述风险，我们提供以下策略供大家参考：

- POS机注册激活机制，即解决收银终端软件被非法安装的问题，又可以在POS机被盗时直接屏蔽掉；
- 请求及响应参数签名机制，防止客户端伪造，及请求篡改；
- 走HTTPS协议，且限制合法根证书，防止中间人抓包、监听、请求重放；
- 限制当天内的订单可以在当时交易的POS机上发起退款，超过一天的只能通过微信支付商户系统进行退款，解决恶意退款问题。

![](/static/images/2017/04/lvpnf4mehjrzzy2t!1200.jpeg)

另外微信支付官方安全团队也在微信支付的开发者文档里面加入了“最佳安全实践”，大家可以自行前往查看。

### 推荐使用微信支付网络监控工具

为了更好的监控商户服务器与微信支付服务器之间的网络质量，微信支付的运维团队提供了一套网络监控工具，通过将监控数据上报到微信支付的运维系统，方便运维人员帮助商户优化链路质量。

该工具的详细使用说明可以看微信支付开发者文档中的说明。

![](/static/images/2017/04/0q1ziizbkr9e9mb9!1200.jpeg)

### 写在最后

综上所述，从无到有搭建一套高可用收银系统要考虑的问题点很多。全部自建成本不低，建议多关注云服务商提供的一些基础能力（BGP高防、BGP网络访问跨地域实时切换、分布式消息队列CMQ、负载均衡CLB、弹性伸缩AS、TDSQL、“云支付”等），尽可能站在云时代的基础设施上来进行高效研发才是更加明智的选择。

再次强调，我们追求的是 “尽可能把支付做成”！

微信支付团队会继续保持对“高可用收银系统”的技术研究，希望可以持续给整个行业输送经验，帮行业提升服务质量，最终让用户享用到更好的移动支付服务。

本文作者：郭润增、唐川鹏、黄东庆、苗俊磊、邵然



