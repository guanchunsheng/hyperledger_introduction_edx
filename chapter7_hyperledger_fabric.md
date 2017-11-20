# 超级账本Fabric入门
## 介绍和学习目标
### 学习目标
- 理解超级账本Fabric v1.0的基础
- 通过Fabric上的一个实例进行演练和分析
- 讨论Fabric结构中的关键组件，包括客户端，peer，排序服务和成员服务管理（MSP）
- 通过javascript SDK构建一个演示网络和简单应用
- 讨论chaincode（Fabric的智能合约）并浏览一个示例
- 参与到框架的讨论和开发中

## 金枪鱼产业链实例
援引[世界经济论坛](https://www.weforum.org/agenda/2017/05/can-technology-help-tackle-illegal-fishing/):
> “非法、隐瞒、逃脱监管（IUU）渔业每年捕捞大约2600万吨，合240亿美元的海产品”

---
### 访谈
Hey, everyone! I hope things are going swimmingly!
Yeah, let's just keep swimming right through this chapter.
Globally, three trillion dollars are spent every year on marine coastal resources and industries.
Marine fisheries employ over 200 million people, from fishing, to processing, to shipping, and sales.
As much as 40% of our oceans are heavily affected by human activities like illegal fishing.
Every year, five million tons of tuna, with an estimated value of forty billion dollars, are sold.
This is a huge industry, and one that could benefit greatly from innovation and transparency.
With the Tuna 2020 Traceability Declaration in mind, our goal is to eliminate illegal, unreported, and unregulated fishing.
We will use Hyperledger Fabric to bring transparency and clarity to a real-world example: the supply chain of tuna fishing.
We will be describing how tuna fishing can be improved, starting from the source, fisherman Sarah, and the process by which her tuna ends up at Miriam's restaurant.
In between, we'll have other parties involved, such as the regulator who verify the validity of the data and the sustainability of the tuna catches.
We will be using Hyperledger Fabric's framework to keep track of each part of this process.
Now, as you read through the demonstrated scenario section, there are two main ideas to pay attention to:
1. There are many actors within the network, and you will see how these actors interact, and how a transaction is conducted.
2. Private channels allow Sarah and Miriam to privately agree on the terms of their interaction,
while still maintaining transparency, so other actors can corroborate and confirm their transaction.
Using private channels, regulators and restaurateurs can confirm whether a particular shipment of tuna was sustainably and legally sourced,
without needing to see the details of the entire journey.
Only the fisherman and the restaurateur are privy to such specific details.
Good luck and let's dive right on into it!
---
**待完成**

## 关键组件和交易流程
---
### 访谈
介绍超级账本Fabric的架构 - Arianna Groetsema

Hello, everybody!
We'll now be going over the architecture of Hyperledger Fabric.
Hyperledger Fabric is so unique, because it allows for modular consensus and membership service.
This means that algorithms for consensus, identity verification are plug-and-play,
resulting in a universal blockchain architecture, that can be applied to most industries or business models.
Channels are another unique feature.
They allow transactions to be private between two actors, while still being verified and committed to the blockchain.
You will also learn about the different roles within a network, how consensus is reached, and other special features.
By the end of this section, you will understand how a transaction is performed between two actors and what exactly occurs within a network during a transaction.
Good luck, and I'll see you in the next section!

---
### Fabric网络中的角色
3中不同类型的角色：
- 客户端
- Peer
- 排序服务

**客户端**：在网络中代表参与人来提起交易的应用程序
**Peer**：Peer维护者网络运转，并保存着账本的一份拷贝。有2种类型的peer，**背书peer**和**提交peer**。但其实两者并不是泾渭分明的，背书peer同时也是一种特殊的提交peer。所有的peer都向分布式账本去提交区块。
- *背书peer* 对交易进行模拟并为它背书
- *提交者* 检查首先背书和交易结果，然后将交易提交到区块链
**排序服务**：排序服务接受背过书的交易，将它们排好顺序加入区块，之后发送给提交peer

### 如何达成共识
在分布式账本系统中，**共识** 的意思是就下一次加入账本中的这组交易达成一致的过程。在Fabric中，共识分别通过3个步骤来实现：
- 交易背书
- 排序
- 验证和提交
这3个步骤维持着网络策略得到贯彻。通过分析交易流程我们会看到这些步骤是怎么实现的。

### 交易流

#### 步骤1
在Fabric网络中，交易始于客户端应用提出交易提案，也就是向背书peer提出一个交易。
![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/21431955acd5b7888ca8d393c94deaf8/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Key_Components_-_Transaction_Proposal.png)
**客户端应用** 通常称为**applications** 或者 **clients**，人们需要通过它们来与区块链网络沟通。应用开发者可以通过应用SDK来使用超级账本Fabric网络。

#### 步骤2
每个背书peer对提案交易进行模拟执行，而不会真的更新账本。背书peer会捕获读（R）和写（W）数据的集合，称为**RW集**。RW集会捕获交易模拟执行过程中对当前世界状态的读取和写入信息。然后背书peer对RW集进行签名，返回给客户端应用，用于后续的交易流程。
![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/13e5a6a80c0e150f46d45ec0634b86b8/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Transaction_flow_step_2.png)
背书peer必须保存智能合约，这样才能模拟交易提案的执行。

**交易背书**
A transaction endorsement is a signed response to the results of the simulated transaction. The method of transaction endorsements depends on the endorsement policy which is specified when the chaincode is deployed. An example of an endorsement policy would be "the majority of the endorsing peers must endorse the transaction". Since an endorsement policy is specified for a specific chaincode, different channels can have different endorsement policies.

所谓的交易背书就是对一个经过模拟的交易进行签名并返回的过程。交易背书的方法取决于部署chaincode的时候指定的背书策略。比如说“必须有多数背书peer对本交易完成背书”。因为背书策略是针对具体chaincode的，所以不同的通道可以有不同的背书策略。

#### 步骤3
客户端应用把经过背书的交易，连同RW集，提交到排序服务。排序在整个网络开展，各个客户端应用的不同交易和RW集一起进行排序。
![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/b6e7b13624d1cff4152e2c223538c355/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Transaction_flow_step_3.png)

#### 步骤4
排序服务把背书后的交易、RW集以及其他信息一起加入到区块中，最后把区块交给提交peer。
![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/eeb54ce57f8a6018443e22f34b3ebad9/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Transaction_Flow_Step_4.png)
**排序服务** 是由排序者的集群构成，并不处理交易、智能合约，也不会管理账本。排序服务仅仅接收经过背书的交易，然后对交易做出顺序上的调整，确定写入账本的交易的顺序。Fabric v1.0架构设计的时候就考虑到可以接受不同的排序实现（Solo，Kafka，BFT等），把排序服务做成了可插拔的组件，所以Fabric是模块化的。默认的排序服务是Kafka。

#### 排序
---
##### 访谈
排序服务 - Chris Ferris
字幕开始。跳转至结尾。
The ordering service is actually something that we conceived of as a function of the initial rollout of Fabric 0.6, last year,
in the sense that... we determined that, in order to improve the performance of the consensus computation,
that, if we separated out the ordering aspects of consensus, where typically, whether it's Bitcoin or Ethereum, the minors are determining the order of transactions in a block,
if we instead make that in an independent service, and apply the fault tolerance to the ordering service itself,
we can actually get a significant improvement in performance and throughput of the overall system.
And so, we've implemented, to date, two ordering services.
One is called SOLO - it's really a toy; I mean, it's intended to be used for development purposes, or initial testing of new functions, and so forth.
And then, another one is based on an implementation of Kafka.
And, over time, as we go forward, we plan on introducing various forms of fault tolerance too to that ordering service.
And so, the initial one is going to be based on RAFT consensus, which isn't byzantine fault tolerant, but it is crash fault tolerant,
and then, there is ongoing work on something we call Simplified Byzantine Fault Tolerance,
and that, we should have probably in the first half of 2018.
---
*同一个时间表内的交易按照串行顺序保存在同一个区块中*

在区块链网络中，写入共享账本的交易，必须保持一致的顺序。交易的顺序必须保证在提交到网络中时，对世界状态的修改是有效的。不同于比特币区块链，比特币区块链的排序是由PoW时通过解决密码学难题来实现的，也就是挖矿，而Fabric允许机构组织运行网络，来选择最适合网络的排序机制。Fabric的模块性和灵活性使得它很适合于企业应用场景。

Fabric提供了3种可选的排序机制用于对交易顺序达成一致：
- SOLO
- Kafka
- 简化拜占庭容错（SBFT），Fabric v1.0还未包含

**SOLO** ：这只是开发人员用来测试Fabric网络用的，只包含单一排序节点。
**Kafka** ：生产环境建议使用Kafka排序。使用了Apache Kafka这个开源的流处理平台，提供统一化的，高吞吐低延迟实时数据处理能力。使用Kafka的时候，处理的数据也就是经过背书的交易和RW集。Kafka机制提供了对排序的崩溃容错解决方案。
**SBFT** ：表示简化拜占庭容错。这个机制既是奔溃容错的，也是拜占庭容错的，也就是说这个机制在有恶意或者功能失效的节点存在时，也能够达成一致。目前还没有实现这个机制，不过已经在计划中了。

#### 步骤5
提交peer验证交易，检查RW集仍然是符合当前世界状态的。特别是，背书peer模拟执行交易时读取的R数据，和现在的世界状态还是一致的。提交peer验证过交易之后，交易即被写入账本，世界状态根据RW集中的W来更新。
![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/b05e5430900cf5e414e307d2f99de088/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Transaction_Flow_Step_5.png)
如果交易失败，也就是说提交peer发现RW集和当前世界状态不符，那么交易还是会写入区块，但是会被标记成无效，世界状态也不会按照无效的交易进行更新。

提交peer负责把区块加入的共享账本中，并且更新世界状态。他们可以保存智能合约，没有保存也没关系。

#### 步骤6
最后，提交peer异步的通知客户端应用，交易是成功了还是失败了。每个提交peer都会通知应用的。
![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/ba380b73a55eff97c85da3abdc1d86e8/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Transaction_Flow_Step_6.png)

**身份验证**
除了大量的背书、有效性验证和版本检查，在交易流程的每个步骤还会进行身份验证。

在网络各层次结构中会实现接入控制列表（从排序服务到通道），交易提案在不同组件之间传递的时候，负载的数据被反复签名、验证和鉴权。

#### 交易流程总结
It is important to note that the state of the network is maintained by peers, and not by the ordering service or the client. Normally, you will design your system such that different machines in the network play different roles. That is, machines that are part of the ordering service should not be set up to also endorse or commit transactions, and vice versa. However, there is an overlap between endorsing and committing peers on the system. Endorsing peers must have access to and hold smart contracts, in addition to fulfilling the role of a committing peer. Endorsing peers do commit blocks, but committing peers do not endorse transactions.
需要特别注意，网络状态的维护是通过peer实现的，并不是通过排序服务或者客户端应用。正常来说，你需要设计你自己的网络，不同的机器在网络中起到不同的作用。用于排序的机器就不应该同时还承担背书或者提交交易的任务，反之亦然。

但是背书和提交peer是可以重叠的。背书peer必须保存智能合约，并且承担提交peer的任务。背书peer需要提交区块，但是提交peer并不需要背书交易。

Endorsing peers verify the client signature, and execute a chaincode function to simulate the transaction. The output is the chaincode results, a set of key/value versions that were read in the chaincode (Read set), and the set of keys/values that were written by the chaincode. The proposal response gets sent back to the client, along with an endorsement signature. These proposal responses are sent to the orderer to be ordered. The orderer then orders the transactions into a block, which it forwards to the endorsing and committing peers. The RW sets are used to verify that the transactions are still valid before the content of the ledger and world state is updated. Finally, the peers asynchronously notify the client application of the success or failure of the transaction.
背书peer验证客户端应用签名，执行chaincode函数来模拟交易流程。产生的输出是chaincode结果，chaincode需要读出的变量键值对（Read集），chaincode需要写入的变量键值对（Write集）。提案反馈需要发回给客户端应用，附带背书签名。

然后这些提案反馈会发给排序者进行排序。排序者把交易按照顺序排入区块，然后转发给背书和提交peer。RW集用于验证交易是不是仍然有效，验证通过后才能写入账本，更新世界状态。最终，peer异步通知客户端应用，交易成功与否。

### 通道channel
通道允许不同组织使用同一个网络，在同一个网络中管理不同的区块链。只有同一个通道的成员，才能够参与这个通道上进行的交易。换句话说，通道把网络按照不同的利益相关方进行了分区，只有相关利益的人在同一个分区。这个机制通过把交易委托给不同的账本来实现。只有同一个通道上的成员，才能够参与共识过程，网路中的其他成员是看不到这个通道上的交易的。
![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/b23a6aaaa627620a0ab161c556ff87b3/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Key_Components_Channels.png)
上图展示了3个独立的通道：蓝，桔黄和灰。每个通道都有自己的应用、账本和peer。

同一个peer可以属于不同的通道和网络。参与多个通道的peer，在不同的账本上模拟和提交交易。但是排序服务是不区分通道的，在全网进行排序。

注意几点：
- 可以在网络设置中创建通道
- 同一个chaincode逻辑可以应用在多个通道上
- 一个指定用户可以参与多个通道

### 状态数据库
当前状态数据代表了账本中各个资产的最新的值。既然当前状态代表了通道上所有已提交的交易，那么有时候当前状态也称为世界状态。

Chaincode调用就是针对当前状态数据进行交易执行的。为了使这些chaincode交互更有效率，每个资产的最新的键值对都保存在数据库中。这个状态数据库只是链上已经提交的交易的数据索引视图。可以根据这个链在任何时候重建。状态数据库在peer启动的时候，在新的交易接受之前，会自动的恢复（后者根据需要重新生成）。默认状态数据库是**LevelDB**，可以替换成**CouchDB**。
- LevelDB是Fabric默认的数据库，简单的保存键值对
- CouchDB可以替代LevelDB。CouchDB按照Json对象保存数据，可以支持keyed，composite，key range和完整的data-rich查询。

Fabric的LevelDB和CouchDB在结构和功能上是很相似的。两者都支持chaincode操作，比如获取和设置键，基于这些键进行查询。两者都可以通过键的范围进行查询，多个参数可以通过复合键进行查询。但是，CouchDB因为是按照Json对象保存数据的，所以支持对chaincode数据的rich查询，如果chaincode的数据（例如资产）是按照Json保存的。

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/9fa87a2726077cff05169f85584224ac/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/State_Database.png)

### 智能合约
回顾一下，智能合约是计算程序，是用于执行交易、修改账本上存储的数据的逻辑。Fabric的智能合约称为**Chaincode**，使用Go语言写的。

Chaincode是Fabric网络的业务逻辑，在chaincode中指引你如何操作网络中的资产。我们会在后续的 *理解Chaincode* 章节讨论更多细节。

### 成员服务管理（MSP）
MSP是一个组件，其中电议了身份验证、鉴权和网络准入的规则。MSP管理用户ID，对意图加入网络的客户端进行鉴权。这包括了为提出交易的客户端分配密钥的工作。MSP会使用 *认证中心* ，认证中心验证和销毁用户的证书。MSP默认使用的接口是Fabric-CA API，具体组织机构可以实现外部认证中心，如果他们想要这么搞的话。这就是Fabric所谓的模块化。

Fabric支持多种安全架构，可以只用多种外部认证中心接口。所以，单独的一个Fabric网络可以控制在多个MSP之下，每个组织可以各得其所。

**MSP做什么？**
开始的时候，用户使用认证中心进行鉴权。认证中心为应用、peer、背书者、排序者标记身份，验证他们的密钥。通过 *签名算法* 和 *签名验证算法* 生成签名。

签名的生成始于 *签名算法*，各实体使用跟各自身份相关的密钥生成背书信息。生成的签名是一串字节，绑定到具体的身份。然后 *签名认证算法* 根据身份，背书信息和签名，进行验证。如果签名字节串包含输入背书信息的有效签名，验证通过；无效就拒绝。如果输出是通过，那么用户可以看到网络中的交易，与网路中的其他角色进行交易。如果输出是拒绝，那么用户鉴权失败，不能向网络提交交易，也不能看到之前的交易。

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/2fe3f7dc2fa52699a96ef7948432113b/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/The_role_of_membership_service_provider.jpg)

### Fabric-认证中心（CA）
通常，认证中心管理permissioned区块链的证书登记。Fabric-CA是超级账本Fabric的默认认证中心，处理用户身份的注册。Fabric-CA负责发布和废除登记证书（E-Certs）。当前Fabric-CA只是发布E-Certs，提供长期的身份证书。登记认证中心（E-CA）发布的E-Certs，给Peer节点赋予身份，赋予他们加入网络并提交交易的权限。

## 安装Fabric
## 理解chaincode
## Chaincode演练
## 编写应用
## 参与Fabric社区
## 结论
