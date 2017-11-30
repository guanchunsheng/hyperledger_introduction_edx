# 5. 超级账本Iroha项目入门
## 5.1 介绍
---
**超级账本Iroha介绍**
Alexandra 和 Arianna Groetsema
Hi everyone! We are the content creators for the Hyperledger Iroha chapter.
In this chapter, you will learn about Hyperledger Iroha version 0.95, what makes it unique, and how to get started with this framework.
This framework is a blockchain platform implementation, designed to be easily incorporated into other distributed ledger technologies.
Hyperledger Iroha features an architecture written in C++, an emphasis on mobile and web development, and a new consensus algorithm called Sumeragi.
Hyperledger Iroha offers many reusable C++ components, that can be used by many different blockchain platforms.
Hyperledger Iroha is dedicated to compiling a library of reusable modular components that enhance existing distributed ledger frameworks.
Now, as you go through this chapter, if you're interested in becoming more involved with Hyperledger Iroha,
there are some resources you can check out, like Rocket.Chat, which is similar to Slack, GitHub repos, and others.
Those specific links can be found at the end of this chapter.
Good luck!
---

## 5.2 学习目标
通过本章的学习，你将可以：
- 理解Iroha v0.95版的基础知识
- 讨论Iroha架构中的关键组件，包括共识算法YAC（Yet Another Consensus），peers，客户端和账本块存储*Ametsuchi*
- 加入Iroha框架的讨论和开发中去


## 5.3 关键组件
超级账本Iroha是一个区块链框架，是Linux基金会主办的超级账本项目之一。超级账本Iroha最初是由Soramitsu、日立、NTT数据一起建立的。Iroha的设计目标是为需要分布式账本的项目提供简单易用的基础设施。Iroha具有构建简单、时髦、域驱动的C++设计，侧重移动应用开发，以及YAC的一致性算法。

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/94f08731c2a2a792a7a20c298839aa38/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Iroha_3_sm.png)

---
**Iroha关键组件**
Alexandra 和 Arianna Groetsema
Hyperledger Iroha seeks to contribute three main goals:
1. Provide an environment for C++ developers to contribute to Hyperledger.
2. Provide infrastructure for mobile and web application support,
and 3. Provide a framework to introduce APIs and a new consensus algorithm that can potentially be incorporated into other frameworks in the future.
The core architecture of Hyperledger Iroha was inspired by Hyperledger Fabric, which we will cover in Chapter 7.
Hyperledger Iroha's creators have emphasized the importance of this framework in fulfilling the need for user-friendly interfaces.
In doing so, they've created a framework with many defining features,
including a simple construction, a modern C++ design, with an emphasis on mobile application development,
and a new chain-based Byzantine Fault Tolerance consensus algorithm, called Sumeragi.
Perhaps the most defining characteristic of Hyperledger Iroha is its ability to be freely interoperable with other Hyperledger projects.
The open source iOS Android and JavaScript libraries allow for developers to conveniently create functions to perform common operations.
Good luck, and let's get into it!
---

### 5.3.1 架构概览
在深入Iroha的关键部件之前，从总体上了解这个框架是很重要的。下图显示了组成Iroha的组件的分层结构视图。共有四层，分别是：API、Peer交互、链式业务逻辑和存储。
![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/cb033d180ecb4eb288f2f0a489195e6c/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Iroha-architecture.png)

包括组件：
- **Model**，系统实体
- **Torii（门）**，为客户端提供输入输出接口。它是一个单一的GRPC服务器，客户端通过门与peer在网络上交互。客户端的RPC调用是非阻塞的，所以torii是异步服务器。这两个命令（交易）和查询（读访问）都是通过这个接口执行的
- **Network**，包括与peer网络的交互
- **Consensus**，负责对网络中的区块链内容达成一致。Iroha所使用的共识机制是YAC（另一个共识），这是一个基于块的投票的实用拜占庭容错算法
- **Simulator**，生成一个临时的存储快照，通过对该快照执行交易来验证它们，并形成一个验证的建议，该建议仅包含有效的交易
- **Validator**，检查业务规则，检查交易或查询的有效性（格式正确）。Iroha发生的两种不同的类型的验证：
  - **Stateless验证**，验证的更快速的形式，对交易的模式和签名进行检查
  - **Stateful验证**，验证的比较慢一点的形式，它检查权限和当前世界状态视图，这是链的最新和最真是的状态，以查看所需的业务规则和策略是否可行。例如，帐户有足够的资金进行转账吗？
- **Synchronizer**，帮助新接入系统的peer或者从系统中暂时脱离的又重新接入的peer做同步
- **Ametsuchi**，账本块存储，由一个块索引（目前是Redi是）、块存储（目前是纯文件），和一个世界状态视图组件（目前是PostgreSQL）组成

### 5.3.2 网络成员
在超级账本Iroha网络中有3个主要的成员：

- **Client**，它们能够：
1. 查询可以访问的数据
2. 执行状态修改动作，即所谓的“交易 transaction”，其中包含的原子操作叫做“命令 command”。比如，在一个单个的交易中，一个用户向3个人转账（3个独立的命令）。但是，如果他/她余额不足，整个交易都会取消。
- **Peer**，本地复制一份账本，并且维护账本的当前状态。一个Peer是网络中的一个单独的实体，且具有一个地址，即身份，还有自己的信用。超级账本Iroha的设计，允许Peer是一台计算机或者一定规模的集群，也就是说账本的存储、索引和验证可以使用不同的计算机，基于P2P的逻辑。
- **排序服务**，把交易按照一定顺序排列。排序服务可以使用几个不同的算法。Kafka是一个很理想的选择。需要指出的是如果使用Kafka或者其他任何分布式方案，那么它们都是集群的；否则不能抵御单点失败。

### 5.3.3 交易流基础
**步骤1**
一个Client向**Torii**门创建并发送一个交易，然后这Torii把这个交易路由到负责处理stateless验证的peer上。
![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/31cba94d9a190c0a12ab6bac4c891e14/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Step_1_of_Iroha_Transaction_Flow.png)

**步骤2**
peer完成stateless验证后，这个交易首先发送到排序门，这里负责选择连接到**排序服务**的正确策略。
![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/21102f3a868cc94b8157d7871ca30192/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Step_2_of_Iroha_Transaction_Flow.png)

**步骤3**
排序服务把交易进行排序，然后转发交易到共识网络的peer，用**proposals**的形式。一个proposal是一个未经签名的由排序服务共享的块，包含一组排好顺序的交易。排序服务积攒到足够的交易或者自从上一次proposal提出后经过了足够的时间，才会转发proposal。者是为了防止排序服务发出空白的proposal。
![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/dd7b06c38b34545769fb70f4e5a1a61a/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Step_3_of_Iroha_Transaction_Flow.png)

**步骤4**
每一个peer都在simulator中验证proposal的内容（stateful验证），然后创建区块，只包含经过验证的交易。这个区块然后发送给*共识门*，由后者执行YAC共识逻辑。
![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/91303543f6393c61426a9b216083bed8/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Step_4_of_Iroha_transaction_flow.png)

**步骤5**
根据YAC共识逻辑，会选择出一组peer以及一个leader。每个peer通过对proposal签名并发送区块给leader完成投票。
![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/00da4d62048b9bdf0b331629946b3573/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Step_5_of_Iroha_transaction_flow.png)

**步骤6**
如果leader收集到了足够的经过签名的proposal区块（比如，超过2/3的peer），那么leader开始发出**commit消息**，表明这个区块应该添加到参与共识的所有peer的链上。一旦commit消息发出后，这个提案的区块就会经由*synchronizer*追加到每个peer的链上。
![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/118a771edf63220195e924a7154eaedd/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Step_6_of_Iroha_Transaction_flow.png)

### 5.3.4 YAC共识算法
超级账本Iroha现在采用的共识算法叫做 **YAC (Yet Another Consensus)** ，这是一个基于为区块哈希投票的算法。共识过程包括获取经过验证的区块，与其他区块商定是否commit，在peer之间传播commit。YAC算法具有2个功能：排序和共识。

排序负责给所有的交易排顺序，把它们打包进proposal，然后向网络中的peer发送这个proposal。排序服务是排定交易顺序和广播proposal的端点。排序服务不负责执行交易的stateful验证。

**注意：** 现在排序服务做排序的时候是单点失败的，所以Iroha不是crash容错的，也不是拜占庭容错的。

共识负责就同一个proposal中的区块达成一致。

验证部分是交易流程中非常重要的一环，但是它跟共识并不是在一起的流程。

#### 共识达成步骤
**步骤1**
排序服务把proposal共享给所有的peer。**Proposal** 是由排序服务创建并共享给网路总所有peer的未经签名的区块。其中包含一组排好顺序的交易。

**步骤2**
peer计算验证过的proposal的哈希，并且签名。得到 **<Hash, Signature>** 元组，称为**投票**。

**步骤3**
基于上一步创建的哈希，每个peer计算出一个排序列表或者peer的顺序。要做到这一步，排序功能需要了解网络中所有投票peer的信息，而且是基于提案区块的哈希进行的。列表中的第一个peer称为**leader**。leader负责收集其他peer的投票并且发出commit消息。

**步骤4**
每个peer都进行投票。leader收集所有的投票并就某个哈希决定是否绝对多数通过。leader发出包含所提交区块的投票信息的commit消息。这个响应就称为**commit**。

**步骤5**
接收到commit后，peer对这个区块进行验证并实施到账本中。至此，共识完成。

#### 共识失败的情况
我们已经介绍了成功达成共识所需的步骤，但是还有一些情况会导致共识失败。下面我们介绍一些共识失败的情况：leader失效和排序的交易无效。

对于leader失效的情况，leader在收集投票的时候可能会不公正，或者leader响应commit的时间太长了。这样的情况，其他peer会为从leader接收commit响应设定一个超时时间。如果超时了，那么peer列表的下一位会成为新的leader。

如果排序服务产生的交易是无效的，那么排序服务转发的交易无法通过stateless验证。peer会把这些交易从proposal中去掉，然后只对其余的交易进行哈希计算。

### 5.3.5 移动库
Iroha最标致性的一个特点就是对移动库的聚焦。

Iroha的一个主要目标是就构建应用方便使用的分布式账本系统。为了达成这个目标，Iroha为**IOS, Android和Javascript** 提供了开源软件库。这些库通过灵活的API函数，为包括但不限于Iroha的网络提供了简单的兼容性。

如果你想要进一步了解这些库，那么可以在github上看到完全开源的信息：
- Android: https://github.com/hyperledger/iroha-android
- IOS: https://github.com/hyperledger/iroha-ios

### 5.3.6 与Fabric和Sawtooth的关系
超级账本未来的一个主要目标就是减少孤立的项目，让更多的库可以当作组件来共享。Iroha期望能够为别的超级账本项目提供以下C++组件：
- YAC共识库
- Ed25519数字签名库
- SHA-3 哈希库
- Iroha交易序列化库
- P2P通信库
- IOS库
- Android库
- JavaScript库
- 区块链浏览器/数据可视化套件

---
**与MakotoTakemiya的Iroha讨论**
> Hi! I'm Robert Schwentker, and this is "Introduction to Hyperledger".
> Today, we'll be talking about Iroha.
> Hi! I'm Makoto Takemiya, the Co-CEO and Co-Founder of Soramitsu. We are the original developers for Iroha.
> We created Iroha last summer, in 2016,
> and then, in 2016 in the fall, we presented it to the Hyperledger Project.
> It was accepted in October as an incubation project.
> What is Iroha?
> Iroha is a distributed ledger platform.
> We created it to be as simple as possible for developers to use, and also, to be highly responsive for mobile and other user-facing applications.
> So, other projects under the Hyperledger umbrella, they focus on enterprises or enterprise systems.
> We're focused more on the user-facing applications.
> What distinguishes Iroha from Fabric?
> So, Fabric is a really large and complex system, and it's great for enterprises that want to manage many applications on a distributed ledger, or even conglomerates of many companies.
> Iroha, it's really focused much more on companies or consortia creating applications that users can use.
> So, really, it's focused on the end-user, and creating applications that are as fast and responsive as possible.
> Can I use Iroha with Fabric?
> In the future, we really want to make that happen.
> So, there's lots of research being done in the field of inter-ledger transactions, and two way pegs, and things like that, sidechains,
> and there's a lot of really interesting research being done in that field, so we want to eventually incorporate that into Iroha.
> Hopefully, by the end of this year, 2017, we will see.
> What are some of the interesting libraries you have for Iroha?
> So, we have the core system. This is called Iroha on GitHub.
> We also have different libraries to help application developers.
> So, we have an Android library, we have an iOS library, and a Javascript library.
> Actually, someone recently made a Scala library.
> And we also have a Python library, as well.
> So, these are different libraries, or SDKs, that developers can use to quickly make applications,
> and just to take out some of the guess work, so they don't make basic mistakes.
> What do you think is one of the most interesting applications built on top of Iroha?
> Right now? At this point, there's not too many applications built.
> We did have a hackathon at the beginning of March in 2016, where, you know, six teams kind of built different applications,
> and one of the cool applications was a kind of decentralized library, like book lending type, and the cool thing about that was,
> you don't have to return a book to the library every time, you can just do a peer-to-peer, and give the book to your friend, your friend scans the QR code,
> and then, the library can keep track of who has this book at any time.
> And those are really interesting use cases, I think.
> What do you think is a use case that could scale up, would be very interesting for a group to build on Iroha?
> So, we're really interested in things like digital currencies and settlement.
> So, actually, just recently [December 2016], we started a project at the University of Izu, to create a campus currency, that's useful in the cafeteria on campus and also at the store,
> and, right now, it's just in the test phase, but it's really... if we could build something where we can expand that to the surrounding economic around all this...
> so if we can do that, you know, it can scale to something that thousands, or tens of thousands of people can use every day.
> From a business person's perspective, what's most compelling about Iroha?
> What's most compelling is probably, I think, our permission model.
> So, a lot of the systems, up to this point, haven't really given too much thought into assistive administration or to the different realities associated with that.
> Yet, the concept of the root user group, and then, also, domains, that manage who can create assets, who can transfer assets, different things like that,
> so, that's something that can allow you to fine-tune the administration and guarantee security of asset management.
> If you look a couple of years into the future, how would you imagine Iroha has evolved?
> So, yeah, in two years... I think what we'll see is in the Hyperledger Project, instead of having many different, you know, monolithic silos of different projects,
> like Fabric, or Iroha, or Sawtooth Lake, I think we'll see much more collaboration between them.
> So, Iroha will probably be a system that kind of has a protocol that, you know, carries on our philosophy of making things really simple, and be responsive,
> but, at the same time, can operate with the other projects in the systems, as well, or even systems like Bitcoin, for instance.
> Bitcoin is successful in its use case, being an open system that anyone can join, so it makes sense to try to build protocols to interoperate with it.
> So, interoperability is, I think, the main go within the next two years.
> Great! Thanks very much, Makoto!
> Thanks!
---

## 5.4 加入Iroha社区
Iroha是一个开源项目，其中的想法和代码都是可以公开讨论，创建和审议的。有很多方式加入Iroha社区，这里分享一些方式来参与其中，可以从技术的角度也可以从想法/问题创建的角度。

捏可以通过Github参与Iroha社区。所有的代码都放在了Github上，可以fork也可以查看：
- https://github.com/hyperledger/iroha
- https://github.com/hyperledger/iroha-ios
- https://github.com/hyperledger/iroha-android
- https://github.com/hyperledger/iroha-javascript
- https://github.com/hyperledger/iroha-python
- https://github.com/hyperledger/iroha-scala
- https://github.com/hyperledger/iroha-dotnet
- https://github.com/hyperledger/iroha-api

**Rocket.Chat和邮件列表**
使用Linux基金会ID参与Rocket.Chat的交流：
- https://chat.hyperledger.org/channel/iroha
- https://chat.hyperledger.org/channel/iroha-smartcontracts

邮件列表：
https://lists.hyperledger.org/mailman/listinfo/hyperledger-iroha

## 5.5 结论
**Iroha API文档**
Iroha团队正在创建API文档。如果你有兴趣阅读和自行尝试，可以访问 https://hyperledger.github.io/iroha-api/#overview

---
**结论**
> Alexandra Groetsema
>
> 恭喜! 你已经按成了Iroha章节的学习。
>
> 我们希望你对与Iroha有了更深入的了解，并且有兴趣在你的分布式账本方案中使用Iroha。
> 
> 如果你想要更多的参与开源项目，请随意在Rocket Chat上进行讨论，在Github上创建Issue。
> 
> 敬请期待，不久的将来会有更深入的介绍Iroha的另一个课程。
> 
> 下章见。
---

- [**目录**](README.md)
- [**上一章 环境准备**](chapter4_prepare.md)
- [**下一章 超级账本Sawtooth项目入门**](chapter6_hyperledger_sawtooth.md)
