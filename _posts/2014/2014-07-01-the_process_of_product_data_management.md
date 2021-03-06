---
layout: post
title: "流程化产品数据运营11步"
date: 2014-07-01
author: 数码林网站分析博客
categories: IT
tags: EC
---

我的一部分工作是数据运营，经常被理解为只做一些数字的研究，做些原因分析，其实这只是数据运营工作的一小部分，数据最终是为产品服务的，数据运营，重点在运营，数据是工具。

数据运营是做什么的？个人的理解是：制订产品目标，创建数据上报通道和规则流程，观测产品数据，做好数据预警，分析数据变化原因，根据分析结果优化产品和运营，并对未来数据走势做出预测，为产品决策提供依据，在产品策划与运营中融入数据的应用。

通俗点说，就是搞清楚以下5个问题：

1. 我们要做什么？——目标数据制订；
2. 现状是什么？——行业分析，产品数据报表输出；
3. 数据变化的原因？——数据预警，数据变化的原因分析；
4. 未来会怎样？——数据预测；
5. 我们应该做什么？——决策与数据的产品应用；

下图是目前我在数据运营工作中推行的工作流程，供大家参考：

![数据分析](/static/images/2014/07/23124964E-0.jpg "数据分析")

### 第1步，制订产品目标。

这是数据运营的起点，也是产品上线运营后进行评估的标准，以此形成闭环。制订目标绝不是拍脑袋出来的，可以根据行业发展，竞品分析，往年产品发展走势，产品转化规律等综合计算得出。产品目标的表现，往往是一个关键数字，例如在2013年12月，某产品日均登录用户数达到100万，制订目标常常用SMART原则来衡量，这里不赘述。

### 第2步，定义产品数据指标。

产品数据目标是反产品健康发展的某一个具体的数字，数据指标则是衡量该产品健康发展的多种数据。例如：

- PV, UV, VV, YV
- ARPU（Average Revenue Per User）
- Attrition rate
- PCU
- DAU、MAU、DAU/MAU
- Entry Event
- Exit Event
- K Factor
- Lifetime Network Value
- Re-Engagement
- Retention

我们根据产品目标来选择数据指标，例如网页产品，经常用PV、UV、崩失率、人均PV、停留时长等数据进行产品度量。定义产品指标体系，需要产品、开发等各个团队达成共识，数据指标的定义是清晰的，并且有据可查，不会引起数据解读的理解差异。

### 第3步，构建产品数据指标体系。

在数据指标提出的基础上，我们按照产品逻辑进行指标的归纳整理，使之条理化。例如一般的客户端产品，我们可以分为帐号体系、关系链、用户状态、用户沟通等四个方面进行数据指标的分类整理。

### 第4步，提出产品数据需求。

产品指标体系的建立不是一蹴而就的，产品经理根据产品发展的不同阶段，有所侧重的进行数据需求的提出，一般的公司都会有产品需求文档的模板，方便产品和数据上报开发、数据平台等部门同事沟通，进行数据建设。创业型中小企业，产品数据的需求提出到上报或许就是1-2人的事情，但同样建议做好数据文档的建设，例如数据指标的定义，数据计算逻辑等。

### 第5步，上报数据。

这个步骤的关键是数据通道的建设，原来在腾讯工作时候，没有体会到这个环节的艰辛，因为数据平台部门已经做了完备的数据通道搭建，开发按照一定规则上报就可以了。现在创业型公司，则是从上报通道开始进行建设，也让我得到更多锻炼提升的机会。其中很关键的一个环节，就是数据上报测试，曾经因为该环节的测试资源没到位，造成不必要的麻烦。

### 第6步到第8步，采集数据，数据存储，数据运算。

每一步都是一门学问，例如采集数据涉及接口创建，要考虑数据字段的拓展性，数据采集过程中的ETL数据清洗流程，客户端数据上报的正确性校验等；数据存储与运算，在大数据时代，更是很有挑战性的技术活，这里也不细说。

### 第9步，获取数据。

就是产品经理，数据分析人员从数据系统获得数据的过程，常见的方式是数据报表和数据提取。报表的格式，一般会在数据需求阶段明确，尤其是有积累的公司，通常会有报表模板，照着填入指标就好了。强大一些的数据平台，则可以根据分析需要，自助的选择字段（表头）进行自助报表的配置和计算生成。数据提取，在做产品运营中，是很常见的需求，例如提取某一批销量较好的商品及其相关字段，提取某一批指定条件的用户等。同样，功能比较完备的数据平台，会有数据自助提取系统，不能满足自助需求，则需要数据开发写脚本进行数据提取。

### 第10步，观测和分析数据。

这里主要是数据变化的监控和统计分析，通常我们会对数据进行自动化的日报表输出，并标识异动数据，数据的可视化输出很重要。常用的软件是EXCEL和SPSS，可以说是进行数据分析的基本技能，以后再分享个人在实际工作中对这两款软件的使用方法和技巧。需要注意的是，在进行数据分析之前，先进行数据准确性的校验，判断这些数据是否是你想要的，例如从数据定义到上报逻辑，是否严格按照需求文档进行，数据的上报通道是否会有数据丢包的可能，建议进行原始数据的提取抽样分析判断数据准确性。数据解读在这个环节至关重要，同一份数据，由于产品熟悉度和分析经验的差异，解读结果也大不一样，因此产品分析人员，必须对产品和用户相当了解。

### 第11步，产品评估与运营优化。

这是数据运营闭环的终点，同时也是新的起点，数据报表绝不是摆设，也不是应付领导的提问，而是切实的为产品优化和运营的开展服务，正如产品人员的绩效，不仅仅是看产品项目是否按时完成，按时发布，更是要持续进行产品数据的观测分析，评估产品健康度，同时将积累的数据应用到产品设计和运营环节，例如亚马逊的个性化推荐产品，例如腾讯的圈子产品，例如淘宝的时光机产品等等。
