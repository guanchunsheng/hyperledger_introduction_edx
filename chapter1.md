# 第一章 探索区块链技术

## 介绍

第一章介绍分布式账本的区块构建，以及区块链的构建。这一章是本教程的基础，用以深入的理解后续章节。需要通过本章的学习来提高对区块链知识的理解。

## 学习目标

学完本章后，应该可以：

* 讨论区块链和分布式账本技术（DLT）
* 探讨permissioned和permissionless类型的区块链及其特点
* 讨论分布式账本技术的各个组件，包括共识算法和智能合约
* 从比较高的层级解释什么是超级账本

## 背景

### 兴味盎然的分布式账本技术

回顾过去半个世纪的计算机技术和结构的发展，可以看到一个趋势，那就是计算能力、存储、基础设施、协议以及代码，从集中式向分布式的转变。

大型计算机是高度中心化的。它们通常集中了所有的计算能力、内存、数据存储和代码。访问大型计算机主要通过”哑终端“来进行，只包含输入和输出，并不存储和处理数据。

随着个人计算机和私有网络的到来，类似的计算能力现在同时分布在服务器和客户端上。这在某种程度上催生了“客户机-服务器”体系结构，它支持关系数据库系统的开发。在大型计算机上维护的海量数据集，现在可以转移到分布式的结构上。数据可以在服务器之间复制，数据的子集可以在客户端上访问和处理，然后再同步回服务器上 。

随着时间的推移，互联网和云计算架构支持从各种计算设备进行全局访问，而大型机主要是针对大型企业和政府的需求。尽管这种“云架构”在硬件方面是分散的，但它产生了应用程序本身的集中化，比如脸书，推特和谷歌等。

目前，我们正见证从集中式的计算、存储和处理向分布式体系结构和系统的转变。

根据[Muneeb Ali](https://medium.com/@muneeb/the-next-wave-of-computing-743295b4bc73) 所说，这些系统的目标是：

> ”将数字资产的控制明确的交给终端用户，无须相信任何第三方服务器和基础设施“

分布式账本技术是促成这个转变的关键性创新。

## 分布式账本技术（DLT）

**分布式账本**是一类数据结构，在多个计算设备之间分布，通常位于不同的位置和区域。

**分布式账本技术**包括区块链技术和智能合约。分布式账本技术先于比特币就已经存在，比特币将交易时间戳、P2P网络，密码学和共享算力、一种新的共识算法与分布式账本结合在了一起。

总的来说，分布式账本技术包含3个基本组件：

* **数据模型** - 捕获账本的当前状态
* **交易语言** - 改变账本状态
* **协议** - 在参与方之间就交易是否被账本接受以及按照什么顺序记录，达成共识

## 访谈

Robert Schwentker

> **主持人**：Robert，什么是区块链技术？
>
> **Robert**：区块链是一个P2P的分布式账本，通过共识，结合智能合约系统和其他辅助性的技术组成。
>
> 这些东西在一起可以构成新一代的交易应用，可以建立信任，可问责性和它们核心的透明性，这将简化业务流程和法律约束。
>
> 所有的分布式账本都有第一个区块，或者叫创世区块。每个区块都会包含一个或者多个交易。连接到一个区块链等同于相关的人通过应用连接到这个分布式账本。
>
> 所以一个例子就是钱包。
>
> 一个人可以将数字资产比如密码学货币转账给另一个人，这样资产就从一个人的钱包转移到了另一个人的钱包，然后这个交易就被记录到区块链里。之后这笔分布式账本交易通过P2P网络传递给全网，没有中介机构比如银行或者支付公司来处理这笔交易。
>
> **主持人**：你能给出一个近年来实际在应用的区块链产品么？
>
> **Robert**：当然，区块链通过比特币被大家熟知。比特币区块链从2009年开始就存在了。比特币是一种密码学货币，但有趣的是，人们混淆了2个术语，比特币不等于区块链，区块链也不等于比特币。
>
> 区块链是一种分布式的账本，追踪各种资产，不同于密码学货币比如比特币。这些交易被打包到区块中，每个区块可以包含任意数量的交易。结果就是，区块链网络中的节点或者机器将这些交易打包并且通过网络发出。
>
> **主持人**：你已经提到了区块链在P2P节点中是怎么处理的，那么它们是怎么同步起来的呢？
>
> **Robert**：区块链同步是通过共识的概念完成的，共识是在网络节点之间达成一致的意思。所以最终，网络中的每一个机器都获得了一份正确的区块链拷贝。

## 区块链

根据 [hyperledger.org](https://hyperledger.org/about) 的定义：

> “区块链是一个P2P的分布式账本，通过共识，结合智能合约系统和其他辅助性的技术组成”

**智能合约** - 当系统内的特定条件满足时，可以执行的预定义好的计算机程序

**共识 **- 指的是确保参与方都同意系统的某个状态是真实有效的状态

**区块链**是分布式账本技术的一个特别的术语或子集，由有序的区块链接而成，所以叫“区块链”。

**区块**表示绑定在一起的一组交易，在同一时间加入到链中。在比特币区块链中，矿工节点打包未确认但是有效的交易到一个块中。每个块包含一定数量的交易。在比特币网络中，矿工必须解出一个密码学的难题来获得下一个块的记录权。这个过程称为“工作量证明”，需要很大的计算能力。我们将在共识算法章节讨论工作量证明的问题。区块链技术的简明历史，请参考[这里](https://hbr.org/2017/02/a-brief-history-of-blockchain)。

时间戳是区块链技术的另一个关键特性。每个区块都有时间戳，新的区块都指向前一个区块。结合密码学的**哈希**，这样的带有时间戳的链接在一起的区块，提供了从创世区块开始的，在网络中无法修改已经写入的交易记录的能力。

一个区块通常包含4类元数据：

* 指向前一个区块的索引
* 工作量证明，也叫nonce
* 时间戳
* 这个区块内含的交易的Merkle树

### Merkle树

The word 'tree' is used to refer to a branching data structure in computer science, as seen in the image below. According to Andreas M. Antonopoulos, in the Bitcoin protocol,

Merkle树也叫二进制哈希树，是一种数据结构，按照特别的方式记录一个大型数据集中单个数据的哈希，用于高效验证这个数据集。它是一种防篡改机制，以确保大数据集没有被更改。“树”一词指的是计算机科学中的分支数据结构，如下图所示。Andreas M. Antonopoulos认为，在比特币协议中：

> “Merkle树用来汇总一个区块中所有的交易，对于所有交易组成的集合生成一个整体的数字指纹，对于验证区块中是否包含的特定的交易非常高效”

![](/assets/Bitcoin_Block_Data.png)上图为比特币区块数据。

对Merkel树的更深入讨论，请见[http://chimera.labs.oreilly.com/books/1234000001802/ch07.html\#\_structure\_of\_a\_block\)](http://chimera.labs.oreilly.com/books/1234000001802/ch07.html#_structure_of_a_block%29)

## 访谈

DLT和区块链的区别 - Brian Behlendorf

> **主持人**：分布式账本技术和区跨链的区别是什么呢？
>
> **Behlendorf**：术语分布式账本和区块链的区别在外界看来非常晦涩。对我个人来说，分布式账本是一个非常好的，非常特别的方式来描述这种分布式的数据库。在这个系统中，你、我和其中的每个人都同步的保有一些列交易的拷贝。
>
> 对于这样的数据结构的术语表述就是区块链，但是现在大家都用区块链这个词代表了密码学货币，企业中部署的DLT等。
>
> 所以我认为区块链就像是Internet一样，含义太宽泛了。但是能够在这里介绍DLT甚至智能合约功能这样的新技术，也是极好的。

什么是区块链 - Dave Huseby

主持人：那么，什么是区块链呢？

Huseby：作为超级账本的安全专家，关于区块链我有一些不同角度的看法。

我最初是一个开发者，并且我会用一种对抗的方式去看世界。通常是我的软件和真实世界的对立。

对我来说，区块链无疑为解决困扰我们许多年的问题打开了另一个解决的可能性。可以通过很多方式在分开的各部分之间建立分布式的共识，也允许我们建立一个即时的唯一真实。

过去，我们总是被“一天的时间中到底发生了什么”这样的问题困扰.

In the past, we've always had problems with trying to figure out what really happened over the course of a day.

Banks reconcile their books, inventory systems clear sales and deliveries...

With blockchains, we're recording things in real time, and it provides us with sort of an up-to-date accounting of the state of the world.

So, a lot of these old systems are going away, a lot of the work load management systems are going away. We're not doing batch processing at night anymore.

Instead, we're doing validation in real time, and we always know the state of the world.

This opens up a whole new host of applications that can be based around...

If we knew exactly where every bolt was, could we make sure that there are always bolts at the place where they're going to be installed two minutes before they are needed... things like that...

So, like lean manufacturing is one case...

But, it also applies to every other process in the world where we have to track materials and effort, and money, things like that.

Let me think...

So, the blockchain, for me, actually is just sort of rethinking of our existing business processes around how we can be more effective when we know the state of the world as it is right now,

and we know it in two minutes, and we know it in two hours, rather than having to wait until the end of the day for everything to be reconciled.

This is actually a transition from human scale to computer scale.

This was... all of these processes were done on a daily basis, and things like that,

and with blockchain, we're now... we're doing it instantaneous, because computers have made it that way.

From a security perspective, blockchains are not a silver bullet.

They're not going to replace the existing business logic.

They're more of a log-based system, so recording what has happened, as it is happening,

rather than being the source of what to do with money from a customer, and things like that.

It's not business process type of software.

Because it is a log, and because it is the source of truth, and because a lot of other business logic will be based upon it,

the security concerns on it, related to blockchain, are roughly the same as existing business practices, or business logic.

You have to think of all the members of a distributed ledger as participating in a closed network,

because that's what they are, at least in permissioned networks,

we're not operating on the public Internet, we're not operating in a trustless environment.

It's the exact opposite. We... all the participants in the distributed ledger are known, they're permissioned,

that's where that term comes from, and they all participate as if they are in a private network.

So, when deploying blockchain software, it's typically done behind a firewall,

and, anytime you have multiple organizations participating in the distributed ledger,

we will use tunneling technology and other VPN firewall tricks to make a virtual LAN exist,

so that all of their organizations can talk over it.

Different Hyperledger projects have different trade-offs.

Fabric, for instance, has a more centralized ordering service,

and so, it makes more sense to organize one of those networks into a hub-and-spoke model,

where each peer connects to the ordering service, which is run either on one of the nodes, or on a neutral third party,

and all of the gossip messages that do the consensus mechanism go through a central hub.

Now, that sounds counterintuitive because just distributed ledgers are supposed to be distributed...

they still are, they're distributed in the sense that the blockchain itself is replicated amongst all of the nodes,

and so, your data is much more resilient to disruption... resilient against disruption.

But, the coordination can still be done in a fairly centralized manner.

With Sawtooth, it is not so much the case, right?

The organization of the network for Sawtooth is probably more of like a star model,

where you pick some subset of nodes, and your node connects to those and, as long as all nodes are fairly well connected,

they can all eventually talk to each other, either one hop, or two hops, or three hops away, depending on the size of your network.

With Iroha, they... their topology is a lot different than the others,

because they designed their distributed ledger to function well in a mobile environment,

where a lot of the clients are intermittently connected.

So, they're mobile phones that can be connected and disconnected at random.

So, their structure is more of a layered network, where the end clients' mobile phones submit transactions to servers,

which then, amongst the servers, do the distributed ledger.

And there's no right answer, right?

The business process dictates which one you choose, but the security is pretty much the same in all of these.

We use cryptography and digital signatures to validate all the messages.

So, tampering with messages is essentially impossible.

The immutability of the distributed ledger is guaranteed by the cryptography,

but the nature of the trusted network, of the permissioned network, means that we have to treat it like any other back-office service.

You want to deploy it behind a firewall, you want to use tunneling to connect between nodes across the Internet,

and you want to maintain it, just like any other service that deals with Internet traffic...

you have firewalls, and load balancers, and things like that.

That's all I've got to say about security. And thanks for taking our course.

