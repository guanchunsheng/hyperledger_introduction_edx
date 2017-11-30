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

## 5.4 加入Iroha社区
## 5.5 结论

- [**目录**](README.md)
- [**上一章 环境准备**](chapter4_prepare.md)
- [**下一章 超级账本Sawtooth项目入门**](chapter6_hyperledger_sawtooth.md)
