# 烧麦微服务架构介绍
### 一、解决什么问题

最开始烧麦存在一个Java应用和多个NODEJS项目，Nodejs提供：行情转发、聊天服务、语音与操作转发；Java应用提供API、后台管理系统、分析师平台系统、PCWeb直播前端等集合于一体，这样的单一系统主要存在以下问题：
1. 业务逻辑复杂耦合，开发维护成本高；
>系统复杂度、规模、参与人数都和腐化程度成正比，单纯的靠模块化，后期来看会存在个别模块成为”大怪物“，臃肿不堪，粒度过粗，难以复用。
2. 交付效率和质量低
>在敏捷和持续集成方法论、实践大行其道的现今，迭代的按期交付率、单测覆盖率都不尽如人意，线上问题频发。
3. 非功能需求不达标
>非功能需求指标特指性能、可用性、可扩展性等方面，代码的腐化和缺少维护、重构，以及没有代码洁癖的人污染下，必然导致性能逐渐下降，甚至出现不同资源竞争的短板效应，造成整个系统奔溃，同时一个大怪物的伸缩性较差，不能随意横向扩展某个细分功能点。
---
最主要的是当前的系统架构已不足以支撑业务，多次出现崩溃宕机的情况，服务的不稳定对业务的拓展是一个很大的干扰，并且在业务规划阶段中，后期的并发量会成指数的增长，为满足业务发展，为解决系统瓶颈，需要探讨新的架构和体系对业务进行支撑。
原始架构图：
![image](http://oauqbak0e.bkt.clouddn.com/QQ%E5%9B%BE%E7%89%8720171204202045.png)
### 二、如何选型
在单独应用不足以支撑业务，并出现多次宕机，导致整体应用不可用的情况，需要提供一个解决方案，为此，基于多年服务治理的经验，我提出以SpringBoot为脚手架，SpringCloud为微服务的解决方案，当时来说，国内更为流行的解决方案是阿里的Dubbo，为此，我从各种资料汇总进行了对比：
- [Spring Cloud 与 Dubbo 对比整理（1）](https://my.oschina.net/undefine/blog/835469)
- [Spring Cloud 与 Dubbo 对比整理（2）](https://my.oschina.net/undefine/blog/848589)

经过讨论，最终确认以SpringCloud为微服务实施方案。

### 三、如何分步骤建设
在多次讨论后，确立了微服务化的三个步骤：
1. 架构设计规划
2. 老系统在过程中的定位和演化
3. 落地实施应用

第一阶段：
进行架构设计规划和层次划分，结合垂直拆分、纵向分层的思想拆分为如下三层：
- 基础设施层
- 业务支撑层
- 对外接入层

进行模块划分，根据业务领域抽象建模，对业务的抽象分解，结合水平拆分，横向成簇的原则，从业务复杂度和粒度入手进行模块划分。

第二阶段：
老系统虽然杂乱，但是能满足当前业务，不可能一刀切，在新服务化未完善上线之前，都需要一直保证老系统的可用，为此，通过网关来分离业务，例如新的分析师平台上线后，通过配置，将老系统的请求路径导流到新服务接入的地址，保证服务的可用和迁移，在业务逐渐的服务化后，最终完全切除老系统。

第三阶段：SpringCloud实施的一个好处就是组件的完善性，包括
- 统一配置中心(ConfigServer)
- 智能路由(Zuul)
- 服务中心(Eureka)
- 通信组件(Feign)
- 监控体系
- ..等

在SpringCloud微服务组件非常完善的情况，可以很简单快速的上线基础设施层，根据不同的C端设立接入层，只需要不断的扩充业务支撑层，当然落地过程中，需要的是整个团队的参与，落地实施也同时是一个系统架构演化的过程。

新型架构初步设计图：
![image](http://oauqbak0e.bkt.clouddn.com/20171204204147.jpg)
### 四、微服务化过程的困难和解决方案
在微服务化的过程中，会遇到很多阻力，包括但不限于
- 开发模式的转变
>微服务推动过程中，很多业务开发人员不熟悉不了解新型架构，我们经过多次技术准备和培训，同时，为解决可能的盲目拆分，实行模块服务的责任化，由研发各个开发人员参与，每人维护一块，减少相互扯皮、责任不到人的问题出现。

> 微服务创建规范可见: [点此前往](https://github.com/lmhaoye/smcbdoc/blob/master/microservice/init.md)
- 业务紧急需求
>业务紧急需求和微服务化的过程是产生了比较多的冲突的问题之一，为满足快速上线，很多情况下，只能从老系统修改调整满足业务。

- 运维成本大大增加（部署、监控、损耗资源）
>由开始的单体应用，成长为超过20多个服务化的应用，对运维来说，成本大大增加，为此，与运维讨论规划，包括增加机器、合理规划不同分层服务的部署，为方便发布和水平扩展，使用walle自动部署平台，管理SIT/UAT/线上的所有发布平台，微服务中也加入监控模块，上报应用健康和接口情况到运维平台，方便监控和及时发现问题。
  另外一方面，数据库架构的更换也带来了挑战，不同业务对应独立的数据库，因此业务的拆分也要技术数据的关联模型设计。

- 接口的大量增加
> 服务之间通过接口交互，并没有单体应用之间基于interface的方式可读性和易用性，为此，从两方面入手，第一建立API文档中心，展示暴露的API定义，第二在程序层面，简历Feign的Interface定义，加入代码注册，独立为maven聚合项目，按照项目划分子Module,方便调用方依赖和使用具体业务。

### 五、总结
上面从几个方面阐述了微服务推动的方方面面，当然，并不是推崇服务化，不鼓励单体模式，单体应用本身模块依赖简单，一个发布包、部署于单容器都使得构建应用非常的简单。在业务探索前期，可以搭建一个简单稳定的基于模块化的平台，在体量逐渐扩大后，再去根据实际需要进行拆分，团队也随之变化。到一定程度，进行建设平台化服务，先按照大的粒度拆分，逐步再根据业务再细化。

结合烧麦自身的场景，并且在团队人员有一定基础素质准备情况下，而且老系统的业务支撑已经到达一定瓶颈的地步，进行微服务化的利是大于弊的，在后期实施过程中，也不断论证了这个观点。