# 超级账本Fabric入门
## 1. 学习目标
- 理解超级账本Fabric v1.0的基础知识
- 对Fabric上的例子进行演练和分析
- 探讨Fabric结构中的关键组件，包括客户端，peer，排序服务和成员服务管理（MSP）
- 通过javascript SDK构建一个演示网络和简单应用
- 探讨chaincode（Fabric的智能合约）并演练一个例子
- 参与到框架的讨论和开发中

## 2. 应用案例 - 海洋渔业
援引[世界经济论坛](https://www.weforum.org/agenda/2017/05/can-technology-help-tackle-illegal-fishing/):
> “非法、隐瞒、逃脱监管（IUU）的渔业每年捕捞大约2600万吨，合240亿美元的海产品”

---
### 访谈
> Hey, everyone! I hope things are going swimmingly!
> Yeah, let's just keep swimming right through this chapter.
> Globally, three trillion dollars are spent every year on marine coastal resources and industries.
> Marine fisheries employ over 200 million people, from fishing, to processing, to shipping, and sales.
> As much as 40% of our oceans are heavily affected by human activities like illegal fishing.
> Every year, five million tons of tuna, with an estimated value of forty billion dollars, are sold.
> This is a huge industry, and one that could benefit greatly from innovation and transparency.
> With the Tuna 2020 Traceability Declaration in mind, our goal is to eliminate illegal, unreported, and unregulated fishing.
> We will use Hyperledger Fabric to bring transparency and clarity to a real-world example: the supply chain of tuna fishing.
> We will be describing how tuna fishing can be improved, starting from the source, fisherman Sarah, and the process by which her tuna ends up at Miriam's restaurant.
> In between, we'll have other parties involved, such as the regulator who verify the validity of the data and the sustainability of the tuna catches.
> We will be using Hyperledger Fabric's framework to keep track of each part of this process.
> Now, as you read through the demonstrated scenario section, there are two main ideas to pay attention to:
> 1. There are many actors within the network, and you will see how these actors interact, and how a transaction is conducted.
> 2. Private channels allow Sarah and Miriam to privately agree on the terms of their interaction,
> while still maintaining transparency, so other actors can corroborate and confirm their transaction.
> Using private channels, regulators and restaurateurs can confirm whether a particular shipment of tuna was sustainably and legally sourced,
> without needing to see the details of the entire journey.
> Only the fisherman and the restaurateur are privy to such specific details.
> Good luck and let's dive right on into it!
---

## 3. 关键组件和交易流程
---
### 访谈
介绍超级账本Fabric的架构 - Arianna Groetsema

> Hello, everybody!
> We'll now be going over the architecture of Hyperledger Fabric.
> Hyperledger Fabric is so unique, because it allows for modular consensus and membership service.
> This means that algorithms for consensus, identity verification are plug-and-play,
> resulting in a universal blockchain architecture, that can be applied to most industries or business models.
> Channels are another unique feature.
> They allow transactions to be private between two actors, while still being verified and committed to the blockchain.
> You will also learn about the different roles within a network, how consensus is reached, and other special features.
> By the end of this section, you will understand how a transaction is performed between two actors and what exactly occurs within a network during a transaction.
> Good luck, and I'll see you in the next section!
---

### 3.1 Fabric网络中的角色
3种角色：
- 客户端
- Peer
- 排序服务

**客户端**：在网络中代表人来发起交易的应用程序

**Peer**：Peer维护着网络的运转，并保存着账本的一份拷贝。有2种类型的peer，**背书peer**和**提交peer**。但其实两者并不是泾渭分明的，背书peer同时也是一种特殊的提交peer。所有的peer都向分布式账本提交区块。
- *背书peer* 对交易进行模拟并为它背书
- *提交者* 首先检查背书和交易结果，然后将交易提交到区块链

**排序服务**：排序服务接受背过书的交易，将它们排好顺序加入区块，之后发送给提交peer

### 3.2 如何达成共识
在分布式账本系统中，**共识** 的意思是就下一次加入账本中的这组交易达成一致的过程。在Fabric中，共识分别通过3个步骤来实现：
- 交易背书
- 排序
- 验证和提交

这3个步骤维持着网络策略得到贯彻。通过分析交易流程我们会看到这些步骤是怎么实现的。

### 3.3 交易流

#### 步骤1
在Fabric网络中，交易始于客户端应用提出交易提案，也就是向背书peer提出一个交易。
![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/21431955acd5b7888ca8d393c94deaf8/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Key_Components_-_Transaction_Proposal.png)

**客户端应用** 通常称为 **applications** 或者 **clients**，人们需要通过它们来与区块链网络沟通。应用开发者可以通过应用SDK来使用超级账本Fabric网络。

#### 步骤2
每个背书peer对提案交易进行模拟执行，而不会真的更新账本。背书peer会捕获读（R）和写（W）数据的集合，称为**RW集**。RW集会捕获交易模拟执行过程中对当前世界状态的读取和写入信息。然后背书peer对RW集进行签名，返回给客户端应用，用于后续的交易流程。
![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/13e5a6a80c0e150f46d45ec0634b86b8/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Transaction_flow_step_2.png)

背书peer必须保存智能合约，这样才能模拟交易提案的执行。

**交易背书**
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
> The ordering service is actually something that we conceived of as a function of the initial rollout of Fabric 0.6, last year,
> in the sense that... we determined that, in order to improve the performance of the consensus computation,
> that, if we separated out the ordering aspects of consensus, where typically, whether it's Bitcoin or Ethereum, the minors are determining the order of transactions in a block,
> if we instead make that in an independent service, and apply the fault tolerance to the ordering service itself,
> we can actually get a significant improvement in performance and throughput of the overall system.
> And so, we've implemented, to date, two ordering services.
> One is called SOLO - it's really a toy; I mean, it's intended to be used for development purposes, or initial testing of new functions, and so forth.
> And then, another one is based on an implementation of Kafka.
> And, over time, as we go forward, we plan on introducing various forms of fault tolerance too to that ordering service.
> And so, the initial one is going to be based on RAFT consensus, which isn't byzantine fault tolerant, but it is crash fault tolerant,
> and then, there is ongoing work on something we call Simplified Byzantine Fault Tolerance,
> and that, we should have probably in the first half of 2018.
---

*同一个时间表内的交易按照串行顺序保存在同一个区块中*

在区块链网络中，写入共享账本的交易，必须保持一致的顺序。交易的顺序必须保证在提交到网络中时，对世界状态的修改是有效的。不同于比特币区块链，比特币区块链的排序是由PoW时通过解决密码学难题来实现的，也就是挖矿，而Fabric允许机构组织自行运行网络，可以选择最适合网络的排序机制。Fabric的模块性和灵活性使得它很适合于企业应用场景。

对交易顺序达成一致，Fabric提供了3种可选的排序机制：
- SOLO
- Kafka
- 简化拜占庭容错（SBFT），Fabric v1.0还未包含

**SOLO** ：这只是开发人员用来测试Fabric网络用的，只包含单一排序节点。

**Kafka** ：生产环境建议使用Kafka排序。使用了Apache Kafka这个开源的流处理平台，提供统一化的，高吞吐低延迟实时数据处理能力。使用Kafka的时候，处理的数据也就是经过背书的交易和RW集。Kafka机制提供了对排序的崩溃容错解决方案。

**SBFT** ：表示简化拜占庭容错。这个机制既是崩溃容错的，也是拜占庭容错的，也就是说这个机制在有恶意或者功能失效的节点存在时，也能够达成一致。目前还没有实现这个机制，不过已经在计划中了。

#### 步骤5
提交peer验证交易，检查RW集仍然是符合当前世界状态的。特别是，背书peer模拟执行交易时读取的R数据，和现在的世界状态是否还是一致的。提交peer验证过交易之后，交易即被写入账本，世界状态根据RW集中的W来更新。

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/b05e5430900cf5e414e307d2f99de088/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Transaction_Flow_Step_5.png)

如果交易失败，也就是说提交peer发现RW集和当前世界状态不符，那么交易还是会写入区块，但是会被标记成无效，世界状态也不会按照无效的交易进行更新。

提交peer负责把区块加入的共享账本中，并且更新世界状态。他们可以保存智能合约，没有保存也没关系。

#### 步骤6
最后，提交peer异步的通知客户端应用，交易是成功了还是失败了。每个提交peer都会通知应用的。
![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/ba380b73a55eff97c85da3abdc1d86e8/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Transaction_Flow_Step_6.png)

**身份验证**
除了大量的背书、有效性验证和版本检查，在交易流程的每个步骤还会进行身份验证。

在网络各层次结构中都会实现接入控制列表（从排序服务到通道），交易提案在不同组件之间传递的时候，负载的数据被反复签名、验证和鉴权。

#### 交易流程总结
需要特别注意，网络状态的维护是通过peer实现的，并不是通过排序服务或者客户端应用维持的。正常来说，你需要设计你自己的网络，不同的机器在网络中起到不同的作用。用于排序的机器就不应该同时还承担背书或者提交交易的任务，反之亦然。

但是背书和提交peer是可以重叠的。背书peer必须保存智能合约，并且承担提交peer的任务。背书peer需要提交区块，但是提交peer并不需要背书交易。

背书peer验证客户端应用签名，执行chaincode函数来模拟交易流程。产生的输出是chaincode结果，chaincode需要读出的变量键值对（Read集），chaincode需要写入的变量键值对（Write集）。提案反馈需要发回给客户端应用，附带背书签名。

然后这些提案反馈会发给排序者进行排序。排序者把交易按照顺序排入区块，然后转发给背书和提交peer。RW集用于验证交易是不是仍然有效，验证通过后才能写入账本，更新世界状态。最终，peer异步通知客户端应用，交易成功与否。

### 3.4 通道channel
通道允许不同组织使用同一个网络，在同一个网络中管理不同的区块链。只有同一个通道的成员，才能够参与这个通道上进行的交易。换句话说，通道把网络按照不同的利益相关方进行了分区，只有相关利益的人在同一个分区。这个机制通过把交易委托给不同的账本来实现。只有同一个通道上的成员，才能够参与共识过程，网路中的其他成员是看不到这个通道上的交易的。

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/b23a6aaaa627620a0ab161c556ff87b3/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Key_Components_Channels.png)

上图展示了3个独立的通道：蓝，桔黄和灰。每个通道都有自己的应用、账本和peer。

同一个peer可以属于不同的通道和网络。参与多个通道的peer，在不同的账本上模拟和提交交易。但是排序服务是不区分通道的，在全网进行排序。

注意几点：
- 可以在网络设置中创建通道
- 同一个chaincode逻辑可以应用在多个通道上
- 一个指定用户可以参与多个通道

### 3.5 状态数据库
当前状态数据代表了账本中各个资产的最新的值。既然当前状态代表了通道上所有已提交的交易，那么有时候当前状态也称为世界状态。

Chaincode调用就是针对当前状态数据进行交易执行的。为了使这些chaincode交互更有效率，每个资产的最新的键值对都保存在数据库中。这个状态数据库只是链上已经提交的交易的数据索引视图。可以根据这个链在任何时候重建。状态数据库在peer启动的时候，在新的交易接受之前，会自动的恢复（后者根据需要重新生成）。默认状态数据库是**LevelDB**，可以替换成**CouchDB**。
- LevelDB是Fabric默认的数据库，简单的保存键值对
- CouchDB可以替代LevelDB。CouchDB按照Json对象保存数据，可以支持keyed，composite，key range和完整的data-rich查询。

Fabric的LevelDB和CouchDB在结构和功能上是很相似的。两者都支持chaincode操作，比如获取和设置键，基于这些键进行查询。两者都可以通过键的范围进行查询，多个参数可以通过复合键进行查询。但是，CouchDB因为是按照Json对象保存数据的，所以支持对chaincode数据的rich查询，如果chaincode的数据（例如资产）是按照Json保存的。

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/9fa87a2726077cff05169f85584224ac/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/State_Database.png)

### 3.6 智能合约
回顾一下，智能合约是计算机程序，是用于执行交易、修改账本上存储的数据的逻辑。Fabric的智能合约称为**Chaincode**，使用Go语言写的。

Chaincode是Fabric网络的业务逻辑，在chaincode中指引你如何操作网络中的资产。我们会在后续的 *理解Chaincode* 章节讨论更多细节。

### 3.7 成员服务管理（MSP）
MSP是一个组件，其中定义了身份验证、鉴权和网络准入的规则。MSP管理用户ID，对意图加入网络的客户端进行鉴权。这包括了为提出交易的客户端分配密钥的工作。MSP会使用 *认证中心* ，认证中心验证和销毁用户的证书。MSP默认使用的接口是Fabric-CA API，如果愿意，那么组织机构也可以自己实现外部认证中心。这就是Fabric所谓的模块化。

Fabric支持多种安全架构，可以使用多种外部认证中心接口。所以，单独的一个Fabric网络可以控制在多个MSP之下，每个组织可以各得其所。

**MSP做什么？**

开始的时候，用户使用认证中心进行鉴权。认证中心为应用、peer、背书者、排序者标记身份，验证他们的密钥。通过 *签名算法* 和 *签名验证算法* 生成签名。

签名的生成始于 *签名算法*，各实体使用跟各自身份相关的密钥生成背书信息。生成的签名是一串字节，绑定到具体的身份。然后 *签名认证算法* 根据身份，背书信息和签名，进行验证。如果签名字节串包含输入背书信息的有效签名，验证通过；无效就拒绝。如果输出是通过，那么用户可以看到网络中的交易，与网路中的其他角色进行交易。如果输出是拒绝，那么用户鉴权失败，不能向网络提交交易，也不能看到之前的交易。

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/2fe3f7dc2fa52699a96ef7948432113b/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/The_role_of_membership_service_provider.jpg)

### 3.8 Fabric-认证中心（CA）
通常，认证中心管理permissioned区块链的证书登记。Fabric-CA是超级账本Fabric的默认认证中心，处理用户身份的注册。Fabric-CA负责发布和废除登记证书（E-Certs）。当前Fabric-CA只是发布E-Certs，提供长期的身份证书。登记认证中心（E-CA）发布的E-Certs，给Peer节点赋予身份，赋予他们加入网络并提交交易的权限。

## 4. 安装Fabric
### 4.1 环境准备
要成功安装超级账本Fabric，你需要熟悉Go语言和Node.js，在电脑上安装以下软件：
- CURL
- Node.js, npm包管理器
- Go语言
- Docker和Docker compose

更多信息请参考第4章 环境准备

### 4.2 安装Fabric Docker和工具
下面我们会下载最新发布的Fabric Docker镜像，将它们重新加标签到 **latest**。

在想要存放工具的目录，运行以下命令：

```
$ curl -sSL https://goo.gl/Q3YRTi | bash
```

**注意** ：检查 https://hyperledger-fabric.readthedocs.io/en/latest/samples.html#binaries ，这里有最新的URL地址。

这个命令会下载以下工具，是可执行文件形式，存放在 **bin** 目录内：
- cryptogen
- configtxgen
- confitxlator
- peer

还会拉取Fabric的Docker镜像。运行成功后检查Docker镜像是否拉取成功：

```
$ docker images
![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/ca726400444f52edbc3e54278077f8dd/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Fabric_installation_1.jpg)
```

**注意** ： 拉取下来的镜像是带有版本信息的，运行的时候工具会从 latest 的镜像生成容器，所以需要我们手动的为镜像修改标签：

```
$ docker tag hyperledger/fabric-tools:x86_64-1.0.2 hyperledger/fabric-tools:latest
```

注意替换镜像名称和版本号。

上面图片显示已经修改标签完成，就不需要再修改了。

### 4.3 安装Fabric
记得将下载的bin文件路径加入PATH环境变量。就不需要每次都给出全路径了，通过以下命令修改环境变量：

```
$ export PATH=$PWD/bin:$PATH
```

为了安装本教程使用的工程，需要运行：

```
$ git clone https://github.com/hyperledger/fabric-samples.git
$ cd fabric-samples/first-network
```

### 4.4 开始测试Fabric网络
成功安装Fabric之后，我们可以开始运行一个含有2个成员的简单网络来看看怎么设置Fabric。参考之前给出的金枪鱼渔业的例子，网络包含每条金枪鱼的验证、运输和消费，在渔民Sarah，饭店业主Miriam之间进行的上述资产管理。我们会建立一个简单的只有2个成员的网络，包含2个组织（就是Sarah和Miriam），每个组织都包含2peer和一个排序者。

我们将用Docker镜像启动我们的第一个Fabric网络。还会启动一个容器来运行一个脚本，其中会将peer加入通道，部署和实例化chaincode，然后在chaincode上进行交易。

在 first-network 文件夹下，运行：

```
$ ./byfn.sh -m generate
```

屏幕会显示简短的信息，还有一个 **Y/N** 的提示信息，输入 **Y <回车>** 来继续。

这个步骤会生成各个网路实体的密钥等信息，还有创世区块，用于排序服务；创建通道所需的配置交易。

然后，可以通过以下命令启动网络：

```
$ ./byfn.sh -m up
```

又会有提示信息，还是输入 **Y <回车>** 继续。会打印很多命令行日志，表明容器已经启动，通道建立，peer加入通道成功， chaincode安装完成，实例化完成，在所有peer上调用chaincode，以及其他各种交易日志。

**故障排除说明**

如果签名两个命令运行出现问题，那么可能是Docker镜像出了问题，可以从0开始再做一遍，删除镜像：

```
$ docker rmi -f $(docker images -q)
```

删除后重新准备镜像，参考 *安装Fabric Docker和工具*。

最后，让我们关闭网络。在终端内推出当前执行环境 **Control + c**，然后运行脚本：

```
$ ./byfn.sh -m down
```

会有提示信息，还是输入 **Y <回车>** 继续。这个脚本会删除你的容器，删除密钥信息和通道信息，从Docker注册服务器中删除chaincode 镜像。

这就是这个简单的演示。下一节会更深入的学习chaincode。

---
### 访谈
安装超级账本 Fabric
>    Hi everyone! In this video, we will be covering how to install Fabric and build a test network.
>    Before you start, make sure that you have Go, Node.js, cURL, npm package manager, Docker, and Docker Compose downloaded on your machine.
>    Make sure to open a terminal window, because this is where we will be working in this video,
>    and also, have Docker running on your machine.
>    We are going to start with downloading the Hyperledger Fabric platform-specific binaries,
>    and to do that, I'm going to move into my desktop directory [cd directory], which I am already in.
>    But you can chose any directory that you would like to download the binaries and Docker images in.
>    I'm just choosing the desktop.
>    And you are going to run the following command: 'curl -sSL https://goo.gl/Q3YRTi | bash'.
>    Now, depending on when you are taking this course, I'd recommend checking the Hyperledger Fabric readthedocs page,
>    and make sure under the "Download Platform-specific Binaries"
>    that you use the most updated URL in that command.
>    Great! So, I am going to press 'Enter', and this command might take a couple of minutes to execute,
>    but be patient.
>    This command downloads binaries for cryptogen, config transaction generator, and the Hyperledger Fabric Docker images that I mentioned before.
>    These assets are placed in a 'bin' subdirectory of the current directory that you are in.
>    Great. So, this has finished executing, and we can see the list of Hyperledger Docker images here,
>    and, if we scroll up, you can see all of then being downloaded.
>    You can go through that on your own time.
>    But I want to just point in the direction of the tags here.
>    Now, mine shows 'latest' for each of the ones, so, for 'fabric-ca', there's a 'latest' tag, the tag is 'latest'.
>    So, this is what we want to see.
>    But, if yours doesn't have this, there are some directions in the documentation that show you how to tag each of these with 'latest',
>    because you will need that for some of the following steps.
>    But, I won't need to, because it's already done for me.
>    Ok.
>    Next, we want to install Hyperledger Fabric, and as an additional measure,
>    you may want to add the 'bin' subdirectory to your PATH environment variable.
>    So, these can be picked up without needing to qualify the PATH to each binary.
>    And you can do that by running the following command in the same directory you just downloaded everything: 'export PATH=$PWD/bin:$PATH'.
>    Great.
>    So, now we are going to install the Hyperledger Fabric sample code, which will be used in this tutorial, and that is going to be on GitHub.
>    So, you'll run the following command: 'git clone'... and I am just doing that in my desktop, as well,
>    'https://github.com/hyperledger/fabric-samples.git'.
>    And that will download the repository to my desktop.
>    So, we have that code now.
>    And I am going to cd into 'fabric-samples/first-network'.
>    Great. Let's just 'ls' to see what's in here.
>    Great.
>    We see a lot of yaml files and a 'byfn.sh', which is good.
>    And now, we're ready to start a test Hyperledger Fabric network with this code that we downloaded.
>    So, in the first... make sure you're in the 'first-network' directory, or folder, and run the following command './byfn.sh -m generate'.
>    A brief description will pop up, and you can just type 'y' and 'Enter' to continue,
>    and you'll see the generation of certificates, and other good stuff you can go through and read this on your own if you'd like.
>   But this means that this executed well.
>   Next, you can start the network with the following command... in the same folder, 'first-network' again, './byfn.sh -m up'.
>   Another command will come up, or another question... you can type 'y' and 'Enter' to continue.
>   Now, this command might also take a little bit of time.
>   But logs will appear in the command line, showing containers being launched, and other things.
>   But we'll talk about that when it finishes running.
>   So, this command has finished executing, with this 'END' message here.
>   And we can see, as I mentioned before, there's a lot of logs that appear in the terminal,
>   or in the command line, showing containers being launched, channels being created and joined,
>   chaincode being installed, instantiated, and invoked on all the peers that were created,
>   as well as other various transaction logs, that you can go through and read on your own.
>   Now, if you had trouble with the commands, or you're not seeing something similar to what I am seeing,
>   in the documentation, there is a troubleshooting note I recommend going and looking at that and trying to see if that helps you.
>   So, just to finish up and shut down this network that we've tested out, 'CTRL + C', if you're on a Mac, or exit that execution and run './byfn.sh -m down'.
>   Another message will pop up, press 'y' and 'Enter' to continue.
>   So, this command will kill your containers, remove the crypto material that we downloaded before,
>   and four artifacts, and delete the chaincode images from your Docker Registry.
>   So, it won't delete the 'bin' subdirectory that you downloaded before, but it will just shut everything down and bring it down.
>   And that's it for a simple demonstration.
>   These steps, these simple steps show how we can easily spin up and bring down a Hyperledger Fabric network given the code we have.
---

## 5. 理解chaincode
### 5.1 Chaincode
在Fabric中，Chaincode就是在peer上运行的智能合约。广义上，chaincode 让用户能够在Fabric网络中创建交易，更新资产的世界状态。

chaincode是可以编程的代码，用Go语言编写，在通道中实例化。开发者使用chaincode开发商业合同、定义资产、集中管理分布式应用。chaincode通过应用所调用的交易来管理账本状态。资产通过特定chaincode创建和更新，且不能够被别的chaincode访问。

应用通过chaincode与区块链账本交互。因此，要在通道中每个需要背书交易的peer上安装chaincode。

有2个方法在Fabric中开发智能合约：
- 为单独chaincode实例编写单独的合约
- （更有效的方式），使用chaincode创建分布式应用，管理一个或者多个类型的业务合约的生命周期，让终端用用户在这些应用中对合约进行实例化

#### Chaincode 关键API
编写chaincode的时候，很重要的接口是Fabric的 [ChaincodeStub](https://godoc.org/github.com/hyperledger/fabric/core/chaincode/shim#Chaincode)和 [ChaincodeStubInterface](https://godoc.org/github.com/hyperledger/fabric/core/chaincode/shim#ChaincodeStub)。*ChaincodeStub* 提供了与底层账本交互的接口，比如查询、更新和删除资产。关键的API包括：
- func (stub *ChaincodeStub) GetState(key string) ([]byte, error)
- func (stub *ChaincodeStub) PutState(key string, value []byte) error
- func (stub *ChaincodeStub) DelState(key string) error

**GetState** ：从账本中返回指定key的值。注意不能从Write集获取数据，因为W集上的数据还没有写入账本呢。换句话说，GetState不能从还没有提交的PutState中获取数据。如果状态数据库中找不到指定key，那么返回（nil, nil）。

**PutState** ：把指定的key和其值放入交易W集，作为写数据的提案。直到交易被验证和成功提交之后，PutState才真正修改了账本。

**DelState** ：把指定要删除的key放入交易W集，作为写数据的提案。直到交易被验证和成功提交之后，DelState才真正修改了账本。

### 5.2 Chaincode 程序实例
创建chaincode的时候，需要实现2个方法：
- Init
- Invoke

**Init** ：chaincode收到 *instantiae* 或者 *upgrade* 交易的时候，会调用这个方法。在这里初始化应用状态。

**Invoke** ：chaincode接收到 *invoke* 交易的时候，调用此方法。

作为开发者，必须在chaincode里边实现这两个方法。chaincode必须通过命令 **peer chaincode install** 安装，通过 **peer chaincode instantiate** 命令实例化，然后才能被调用。

交易可以通过 **peer chiancode invoke** 或者 **peer chaincode query** 命令创建。

#### 依赖
让我们逐个语句分析一个Go语言chaincode的例子：

```
package main
import (
  "fmt"
  "github.com/hyperledger/fabric/core/chaincode/shim"
  "github.com/hyperledger/fabric/protos/peer"
)
```

import语句列出了所有构建chaincode所需的依赖。
- fmt 打log用到的Println
- github.com/hyperledger/fabric/core/chaincode/shim 包含了chaincode接口的定义，与账本交互的chaincode的stub，我们在Chaincode关键API 这一节有提到
- github.com/hyperledger/fabric/protos/peer 包含peer protobuf包

#### 结构

```
type SampleChaincode struct {
}
```

没内容，但是这是在Go语言中定义对象/类的起始部分。 *SampleChaincode* 实现了一个管理资产的简单chaincode。

#### Init方法
下面我们会实现Init方法。

Init是chaincode在实例化的时候调用的方法。在我们的例子里，我么你会创建初始的资产键值对，如下：

```
func (t *SampleChaincode) Init(stub shim.ChainCodeStubInterface) peer.Response {
  // Get the args from the transaction proposal
  args := stub.GetStringArgs()
  if len(args) != 2 {
    return shim.Error("Incorrect arguments. Expecting a key and a value")
  }

  // We store the key and the value on the ledger
  err := stub.PutState(args[0], []byte(args[1]))
  if err != nil {
    return shim.Error(fmt.Sprintf("Failed to create asset: %s", args[0]))
  }

  return shim.Success(nil)
}
```

这个Init的实现，接收2个入参，使用 **stub.PutState** 函数向账本写入键值对。**GetStringArgs** 取得并检查参数的有效性，需要参数是一对键值。因此，我们需要检查参数的个数是2.如果不是，那么返回错误。一旦我们接收到正确数量的参数，那么我们开始保存账本的初始状态。为了实现这一点，我们会调用 **stub.PutState** 函数，指定第一个参数是键，第二个参数是这个键的值。如果没有返回错误，我们就从Init方法返回成功。

#### Invoke方法
现在我们探索一下Invoke方法，是在客户端应用提出交易的时候调用的。在我们的例子里，我们会获取给定资产的键值或者更新给定资产的键值。

```
func (t *SampleChaincode) Invoke(stub shim.ChaincodeStubInterface) peer.Response {
  // Extract the function and args from the transaction proposal
  fn, args := stub.GetFunctionAndParameters()
  var result string
  var err error

  if fn == "set" {
    result, err = set(stub, args)
  } else { // assume 'get' even if fn is nil
    result, err = get(stub, args)
  }

  if err != nil { //Failed to get function and/or arguments from transaction proposal
    return shim.Error(err.Error())
  }

  // Return the result as success payload
  return shim.Success([]byte(result))
}
```

客户端可以调用2个基本动作：*get* 和 *set*
- get 方法用来查询和返回现存的某个资产
- set 方法用来创建一个新资产或者更新一个现存资产

开始，我们调用 **stub.GetFunctionAndParameters** 把函数名和参数变量分开。每个交易要么是 set 要么是 get。让我们看下set 方法是怎么实现的：

```
func set(stub shim.ChaincodeStubInterface, args []string) (string, error) {
  if len(args) != 2 {
    return "", fmt.Errorf("Incorrect arguments. Expecting a key and a value")
  }

  err := stub.PutState(args[0], []byte(args[1]))
  if err != nil {
    return "", fmt.Errorf("Failed to set asset: %s", args[0])
  }
  return args[1], nil
}
```

**set** 方法会用给定的值，指定的key，去创建或者修改资产。set 方法会用指定的键值对修改世界状态。如果key是存在的，那么用新的值覆盖以前的值，通过 **stub.PutState** 方法，如果key不存在，那么会创建一个新的键值对。

下面我们看下 get 方法的实现：

```
func get(stub shim.ChaincodeStubInterface, args []string) (string, error) {
  if len(args) != 1 {
    return "", fmt.Errorf("Incorrect arguments. Expecting a key")
  }

  value, err := stub.GetState(args[0])
  if err != nil {
    return "", fmt.Errorf("Failed to get asset: %s with error: %s", args[0], err)
  }

  if value == nil {
    return "", fmt.Errorf("Asset not found: %s", args[0])
  }
  return string(value), nil
}

```

**get** 方法会尝试获取给定key的值。如果一个用传入的不是单个的key，那么返回错误；如果还是单个的key，那么用 **stub.GetState** 方法查询指定key的世界状态。如果这个key还没有加入到账本中（以及世界状态），那么返回错误；如果已经在账本中了，那么从方法中返回这个key的值。

#### main函数
main函数会调用 **start** 函数。**main** 函数在容器中启动chaincode，在实例化的过程中：

```
func main() {
  err := shim.Start(new(SampleChaincode))
  if err != nil {
    fmt.Println("Could not start SampleChaincode")
  } else {
    fmt.Println("SampleChaincode successfully started")
  }
}
```

## 6. Chaincode演练
现在我们大概知道chaincode是怎么编码的了，我们将会实际演练一下，通过在账本中创建一个资产，基于金枪鱼渔业的例子。

有时候代码片段在翻译的时候会丢失，特别是如果上下文没有多大意义的话。为了避免这样的情况，我们调整了用来演示场景的chaincode。本节会分析记录金枪鱼捕捞信息的chaincode，还有查询和更新记录的chaincode。

### 6.1 定义资产属性
这是我们将要记录在账本中的金枪鱼的属性：
- 船只（string）
- 位置（string）
- 日期时间（datatime）
- 持有者（string）

我们创建一个Tuna结构，包含4个属性。结构标签用 **encoding/json** 库

```
type Tuna struct {
  Vessel string ‘json:"vessel"’
  Datetime string ‘json:"datetime"’
  Location string ‘json:"location"’
  Holder string ‘json:"holder"’
}
```

### 6.2 Invoke方法
如前所述，Invoke 方法是客户端应用提交交易提案的时候调用的。在这个方法中，我们有不同类型的交易：
- recordTuna
- queryTuna
- changeHolder

回顾前文，Sarah是渔民，捕捞到金枪鱼时会调用 **recordTuna** 方法

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/fa83f00cd04e8ba4e25d6cfb6035233e/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Invoke_method_recordTuna.png)

Miriam 收到鱼时会调用 **changeHolder** 方法。 Miriam想要查看某条金枪鱼的状体时会调用 **queryTuna** 方法。

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/1e43b46e9a6be77ffbd921e182d03e9d/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Invoke_method_queryTuna_and_changeTuna.png)

监管人员会根据需要调用 **queryTuna** 和 **queryAllTuna** 检查供应链的可持续性。

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/47cd6304d0e5df20df297e5b322cdf73/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Invoke_method_queryTuna_and_queryAllTuna.png)

下面我们会进入到不同的方法中进行探索。但是首先看一下Invoke方法，这个方法会查看第一个参数，这个参数决定调用哪个函数，然后调用适当的chaincode方法。

```
func (s *SmartContract) Invoke(APIstub shim.ChaincodeStubInterface) sc.Response {
  // Retrieve the requested Smart Contract function and arguments
  function, args := APIstub.GetFunctionAndParameters()
  
  // Route to the appropriate handler function to interact with the ledger appropriately
  if function == "queryTuna" {
    return s.queryTuna(APIstub, args)
  } else if function == "initLedger" {
    return s.initLedger(APIstub)
  } else if function == "recordTuna" {
    return s.recordTuna(APIstub, args)
  } else if function == "queryAllTuna" {
    return s.queryAllTuna(APIstub)
  } else if function == "changeTunaHolder" {
    return s.changeTunaHolder(APIstub, args)
  }
    return shim.Error("Invalid Smart Contract function name.")
  }
```

### 6.3 queryTuna方法
渔民，监管人员或者饭店老板会调用这个方法查看某个金枪鱼的记录。带有1个参数，目标金枪鱼的键。

```
func (s *SmartContract) queryTuna(APIstub shim.ChaincodeStubInterface, args []string) sc.Response {
  if len(args) != 1 {
    return shim.Error("Incorrect number of arguments. Expecting 1")
  }

  tunaAsBytes, _ := APIstub.GetState(args[0])
  if tunaAsBytes == nil {
    return shim.Error(“Could not locate tuna”)
  }
  return shim.Success(tunaAsBytes)
}
```

### 6.4 initLedger
向我们的网络加入测试数据：

```
func (s *SmartContract) initLedger(APIstub shim.ChaincodeStubInterface) sc.Response {
  tuna := []Tuna{
    Tuna{Vessel: "923F", Location: "67.0006, -70.5476", Timestamp: "1504054225", Holder: "Miriam"},
    Tuna{Vessel: "M83T", Location: "91.2395, -49.4594", Timestamp: "1504057825", Holder: "Dave"},
    Tuna{Vessel: "T012", Location: "58.0148, 59.01391", Timestamp: "1493517025", Holder: "Igor"},
    Tuna{Vessel: "P490", Location: "-45.0945, 0.7949", Timestamp: "1496105425", Holder: "Amalea"},
    Tuna{Vessel: "S439", Location: "-107.6043, 19.5003", Timestamp: "1493512301", Holder: "Rafa"},
    Tuna{Vessel: "J205", Location: "-155.2304, -15.8723", Timestamp: "1494117101", Holder: "Shen"},
    Tuna{Vessel: "S22L", Location: "103.8842, 22.1277", Timestamp: "1496104301", Holder: "Leila"},
    Tuna{Vessel: "EI89", Location: "-132.3207, -34.0983", Timestamp: "1485066691", Holder: "Yuan"},
    Tuna{Vessel: "129R", Location: "153.0054, 12.6429", Timestamp: "1485153091", Holder: "Carlo"},
    Tuna{Vessel: "49W4", Location: "51.9435, 8.2735", Timestamp: "1487745091", Holder: "Fatima"}, 
  }

  i := 0
  for i < len(tuna) {
    fmt.Println("i is ", i)
    tunaAsBytes, _ := json.Marshal(tuna[i])
    APIstub.PutState(strconv.Itoa(i+1), tunaAsBytes)
    fmt.Println("Added", tuna[i])
    i = i + 1
  }
  return shim.Success(nil)
}
```

### 6.5 recordTuna
Sarah会调用这个方法记录金枪鱼的捕捞信息。需要5个参数（保存进账本的属性）：

```
func (s *SmartContract) recordTuna(APIstub shim.ChaincodeStubInterface, args []string) sc.Response {
  if len(args) != 5 {
    return shim.Error("Incorrect number of arguments. Expecting 5")
  }

  var tuna = Tuna{ Vessel: args[1], Location: args[2], Timestamp: args[3], Holder: args[4]}
  tunaAsBytes, _ := json.Marshal(tuna)
  err := APIstub.PutState(args[0], tunaAsBytes)
  if err != nil {
    return shim.Error(fmt.Sprintf("Failed to record tuna catch: %s", args[0]))
  }
  return shim.Success(nil)
}
```

### 6.6 queryAllTuna
用来获取所有的记录，这里就是加入到账本中的所有的金枪鱼记录。不需要参数，会返回JSON字符串，包含查询结果。

```
func (s *SmartContract) queryAllTuna(APIstub shim.ChaincodeStubInterface) sc.Response {
  startKey := "0"
  endKey := "999"
  resultsIterator, err := APIstub.GetStateByRange(startKey, endKey)

  if err != nil {
    return shim.Error(err.Error())
  }

  defer resultsIterator.Close()
  // buffer is a JSON array containing QueryResults
  var buffer bytes.Buffer
  buffer.WriteString("[")
  bArrayMemberAlreadyWritten := false
  for resultsIterator.HasNext() {
    queryResponse, err := resultsIterator.Next()
    if err != nil {
      return shim.Error(err.Error())
    }
    // Add a comma before array members, suppress it for the first array member
    if bArrayMemberAlreadyWritten == true {
      buffer.WriteString(",")
    }

    buffer.WriteString("{\"Key\":")
    buffer.WriteString("\"")
    buffer.WriteString(queryResponse.Key)
    buffer.WriteString("\"")
    buffer.WriteString(", \"Record\":")
    // Record is a JSON object, so we write as-is
    buffer.WriteString(string(queryResponse.Value))
    buffer.WriteString("}")
    bArrayMemberAlreadyWritten = true
  }

  buffer.WriteString("]")
  fmt.Printf("- queryAllTuna:\n%s\n", buffer.String())
  return shim.Success(buffer.Bytes())
}
```

### 6.7 changeTunaHolder
金枪鱼在供应链中可能会频繁转手，世界状态中可以按照所有权进行更新。这个方法需要2个入参，tuna id和 new holder name。

```
func (s *SmartContract) changeTunaHolder(APIstub shim.ChaincodeStubInterface, args []string) sc.Response {
  if len(args) != 2 {
    return shim.Error("Incorrect number of arguments. Expecting 2")
  }

  tunaAsBytes, _ := APIstub.GetState(args[0])
  if tunaAsBytes != nil {
    return shim.Error("Could not locate tuna")
  }

  tuna := Tuna{}
  json.Unmarshal(tunaAsBytes, &tuna)
  // Normally check that the specified argument is a valid holder of tuna but here we are skipping this check for this example. 
  tuna.Holder = args[1]
  tunaAsBytes, _ = json.Marshal(tuna)
  err := APIstub.PutState(args[0], tunaAsBytes)
  if err 
    return shim.Error(fmt.Sprintf("Failed to change tuna holder: %s", args[0]))
  }
  return shim.Success(nil)
}
```

### 6.8 结论
我们希望现在你已经对怎么组织和编写chaincode有了更好的理解，特别是能够应用在简单例子上了。要看所有的代码片段，请访问Github上的项目： https://github.com/hyperledger/education/blob/master/LFS171x/fabric-material/chaincode/tuna-app/tuna-chaincode.go

## 7. 编写应用
**什么是区块链应用？**

在区块链应用中，区块链会保存系统的状态，以及促成这个状态的所有交易的记录，都永久性的保存在区块链中。向区块链发送交易，是客户端应用的任务。需要用智能合约为业务逻辑（全部或者部分）编码。

**应用和网络如何互动**

应用使用API来运行智能合约。在Fabric中，智能合约称为chaincode。这些合约托管在网络中，通过名字和版本标识。API可以通过软件开发套件，或叫SDK获得。现在，Fabric为开发者提供了3个选项：
- Node.js SDK
- Java SDK
- CLI

**Node.js SDK** ：在这个练习里，我们会使用Node.js SDK https://fabric-sdk-node.github.io/ 与网络和账本互动。Fabric客户端 SDK 为通过API与Fabric区块链进行交互提供了方便。本节会帮助你编写你的第一个应用，从测试Fabric网络开始，然后学习示例智能合约的参数，最后开发查询和更新账本的应用。

更多信息，请访问Fabric Node.js SDK 文档：https://fabric-sdk-node.github.io/tutorial-app-dev-env-setup.html

### 7.1 海洋渔业应用
金枪鱼渔业应用将会演示在金枪鱼供应链中，怎么使用Fabric创建记录和在不同角色间转移金枪鱼所有权。

这个应用会使用Node.js编写。智能合约chaincode会使用我们在上一节所用的代码。与chaincode的交互通过使用gRPC协议与网络上的peer进行通信来实现。gRPC协议的细节由Fabric客户端Node.js SDK负责。

---
#### 访谈
Fabric 金枪鱼渔业应用 Alexandra Groetsema
> So far, in this chapter, we've covered the components of Hyperledger Fabric's framework,
> including the different types of nodes in the network, private channels, and database features.
> We've also installed and spun up our very own test network, deep dived into chaincodes smart contact programming, and gone through a demonstrated example, detailing how Fabric is so unique.
> So, now, we've gotten to the really exciting part, where we combine all of these concepts into a working sample application.
> In this exercise, we'll see how a user can interact with a network through an application that enables users to query and update a ledger.
> Our application handles user interface and submits transactions to the network, which then call chaincodes.
> Fabric currently has three options for developers: a Node SDK, Java SDK and a command line interface or CLI.
> Today, we'll be using Fabric's Node SDK, which makes it easy to use APIs to interact with the Hyperledger Fabric blockchain.
> In a nutshell, reading or writing the ledger is known as a proposal.
> This proposal is built by our application via the SDK, and then sent to a blockchain peer.
> The peer will communicate to its application-specific chaincode container.
> The chaincode will run the transaction.
> If there are no issues, it will endorse the transaction and send it back to our application.
> Our application, via the SDK, will then send the endorsed proposal to the ordering service.
> The order will package many proposals from the whole network into a block, which is then broadcast to the peers in the network.
> Finally, the peer will validate the block and write it to its ledger.
> The transaction is now live.
> By the end of this exercise, we'll be familiar with how to use the Node.js SDK to interact with the network, and, therefore, a ledger,
> and understand how an application chaincode network and ledger all interact with one another.
> Let's get to work!
---

#### 开始 - 启动Fabric网络
下载education工程：

```
$ git clone https://github.com/hyperledger/education.git
$ cd education/LFS171x/fabric-material/tuna-app
```

确保Docker就绪。如果没有安装好环境，请参考第4章 环境准备。

然后，确保已经完成了 *安装Fabric* 小节的内容，然后再开始下面的部分，否则可能会遇到奇怪的错误。

正式开始。首先，清理之前存在的容器，因为它们可能会与本节的容器相冲突：

```
$ docker rm -f $(docker ps -aq)
```

然后通过以下命令启动Fabric网络：

```
$ ./startFabric.sh
```

**故障排除**
如果上面的命令执行后遇到类似这样的报错：

```
ERROR: failed to register layer: rename
/var/lib/docker/image/overlay2/layerdb/tmp/write-set-091347846 /var/lib/docker/image/overlay2/layerdb/sha256/9d3227c1793b7494e598caafd0a5013900e17dcdf1d7bdd31d39c82be04fcf28: file exists
```

那么运行下面这个命令：

```
$ rm -rf ~/Library/Containers/com.docker.docker/Data/*
```

#### 开始 - 启动Web服务
需要安装所需的库，从package.json文件，然后网络上的注册 **Admin** 和 **User** 组件。通过以下命令启动客户端应用：

```
$ npm install
$ node registerAdmin.js
$ node registerUser.js
$ node server.js
```

可以简单的通过浏览器访问 **localhost:8000** 启动客户端。可以看到下面的用户接口：

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/50068889079cd063e30d8bbd6d5fe554/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/fabric-application1.png)

**故障排除**

如果遇到类似的错误：

```
Error: [client-utils.js]: sendPeersProposal - Promise is rejected: Error: Connect Failed
error from query =  { Error: Connect Failed
   at /Desktop/prj/education/LFS171x/fabric-material/tuna-app/node_modules/grpc/src/node/src/client.js:554:15 code: 14, metadata: Metadata { _internal_repr: {} } }
```

那么可以尝试运行以下命令：

```
$ cd ~
$ rm -rf .hfc-key-store/
```

然后从注册Admin开始继续：

```
$ node registerAdmin.js
```

#### 应用程序结构
可以从下图查看Fabric应用的结构：

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/a4867e651f3bb639e579fb3424bd9a62/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/fabric-filestructure.png)

#### 查询所有金枪鱼记录
查询的代码是：

```
// queryAllTuna - requires no arguments
const request = {
    chaincodeId:’tuna-app’,
    txId: tx_id,
    fcn: 'queryAllTuna',
    args: ['']
    };
return channel.queryByChaincode(request);

..src/queryAllTuna.js
```

现在让我们查询一下我们的数据库，我们已经预置了一些数据，因为我们在initLedger方法中hardcode进去了10条记录。在JS中，这个函数不需要入参（args: ['']）。

查询的响应，在网页接口上可以看到这10条预置记录：

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/ae31eb58789457980ae55a97aaee0aa8/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/fabric-queryAll.png)

#### 查询特定记录
JS代码：

```
// queryTuna - requires 1 argument
const request = {
   chaincodeId:’tuna-app’,
   txId: tx_id,
   fcn: 'queryTuna',
   args: ['1']
   };
return channel.queryByChaincode(request);
 
..src/queryTuna.js
```

这个函数有1个入参，这里的例子是 args: ['1']，本例中我们用这个key来查询捕捞信息。

你应该可以看到如下图这样的查询结果，给出捕捞的具体信息：

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/4dbe39c61232ff1bfa939703cf396503/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/fabric-query.png)

#### 改变所有者
JS代码：

```
// changeTunaHolder - requires 2 argument
var request = {
  chaincodeId:’tuna-app’,
  fcn: 'changeTunaHolder', 
  args: ['1', 'Alex'],
  chainId: 'mychannel',
  txId: tx_id
  };
return channel.sendTransactionProposal(request);

..src/changeHolder.js
```

现在让我们为一条给定的金枪鱼指定一个新的所有者。需要2个参数，指定捕捞的key和新的所有者，从代码中可以看到是 ['1', 'Alex']。

你可以在终端中看到类似的信息：

```
The transaction has been committed on peer localhost:7053
 event promise all complete and testing complete

Successfully sent transaction to the orderer.
Successfully sent Proposal and received ProposalResponse: Status - 200, message - "OK", metadata - "", endorsement signature: 0D 9
```

这表明我们从我们的应用通过SDK发出了一个提案，然后peer对它进行了背书和提交，最后账本进行了相应的更新。

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/e36aee4cc0c801b18e855eb1fc234c4b/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/fabric-changeTunaHolder.png)

能够看到所有者确实已经改变了，可以去查看['1']，现在 holder 变成了Alex：

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/c43d7a8103c74fc3c8a0e123889d02ab/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/fabric-changedRecord.png)

#### 增加记录
JS代码：

```
// recordTuna - requires 5 argument
for request = {
  chaincodeId:’tuna-app’,
  fcn: 'recordTuna',   
  args: ['11', '239482392', '28.012, 150.225', '0923T', "Hansel"],
  chainId: 'mychannel',
  txId: tx_id
};
return channel.sendTransactionProposal(request);

..src/recordTuna.js
```

最后我们看下怎么增加一条记录，通过调用 **recordTuna** 函数向账本增加记录。这个函数需要5个参数，为每个新的捕捞信息提供属性。这里的例子是： ['11', '239482392', '28.012, 150.225', '0923T', "Hansel"].

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/5d667e799ccd9caf881bae2b5019883f/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/fabric-createTunaRecord.png)

通过查询所有记录，应该可以看到，新的记录已经成功添加。可以在最下面一行看到刚刚添加的记录：

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/91f75c705cacc4940b721a47d0eb3710/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/fabric-queryAfterCreate.png)

#### 完成
清理我们在这个例子中使用的所有的Docker容器和镜像：

```
$ docker rm -f $(docker ps -aq)
$ docker rmi -f $(docker images -a -q)
```

### 7.2 应用流程基础
#### 流程 

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/a63eaf4dd007c3e65ee63955eccaf5b6/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/fabric-application-flowbasics.png)

1. 开发者创建应用和智能合约
2. 应用通过Fabric 客户端SDK调用智能合约
3. chaincode合约处理以上调用
4. put 或者 delete 命令会完成共识过程，加入到账本的区块链中。
5. get 命令只能从世界状态读取数据，而不能记录到区块链中
6. 应用可以通过API访问区块链信息

#### 举例

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/f01702e7429e967987a62daf274a164b/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/fabric-application-flow.png)

1. 不同的用户（渔民，监管人员，饭店老板）都会通过Node.js应用互动
2. 用户与应用交互的时候，客户端JS向后端发消息
3. 对账本的读写都作为提案（比如查询某次金枪鱼捕捞 *queryTuna* 或者记录金枪鱼捕捞 *recordTuna*）。这样的提案通过我们的应用，经由SDK构建，然后发送给背书peer
4. 背书peer使用应用关联的chaincode智能合约来模拟交易的执行。如果没有问题，那么交易就算背书完成，然后发回给应用
5. 应用将经过背书的提案通过SDK发给排序服务。排序者把来自全网的很多提案打包到区块中，然后把它广播给网络中的提交peer
6. 最后，每个提交peer都会验证区块，然后写到账本里。现在这个交易就算是已经提交了，其后的读操作会看到这个交易的结果

## 8. 参与Fabric社区
Fabric是一个开源项目，其中的想法和代码都是公开讨论、创建和评审的。有很多方法加入到Fabric社区中。下面我们会高亮一些参与的方式，从技术角度或者从创意角度。

---
### 访谈
Fabric的未来 Chris Ferris
> So, the Hyperledger [Fabric] development, obviously, has been ongoing now for about a year and a half.
> We've grown from an initial start of almost exclusively IBM developers, probably about 20 or so initially,
> to the point where we're now over 150 developers collaborating on Hyperledger Fabric, from many companies, and a lot of individuals, and, in fact, a lot of students, as well,
> and so, I think, the future is really to have a much more diverse community of ideas coming into play,
> and helping us plan out what's going on in the next release, helping us fixing bugs, helping us with improving documentation, and so forth.
> And so, I'm really excited about the prospects of having new members come and join us in this journey, in developing permissioned blockchains for the enterprise.
> And so, again, I think, it's really an important opportunity for a lot of people,
> especially as they are beginning their careers, to get involved in an open source project,
> because it can be a real launching pad to a successful career.
---

### 8.1 社区会议和邮件列
可以在Fabric 文档中参加周例会，或Fabric其他会议，参考 [Hyperledger Community Meetings Calendar](https://calendar.google.com/calendar/embed?src=linuxfoundation.org_nf9u64g9k9rvd9f8vp4vur23b0%40group.calendar.google.com&ctz=America/SanFrancisco)

可以参与邮件列表进行技术讨论和查看公告：https://lists.hyperledger.org/mailman/listinfo/hyperledger-fabric

### 8.2 JIRA和Gerrit
如果有bug需要报告，可以通过JIRA提交issue（需要Linux基金会账号访问JIRA）：https://jira.hyperledger.org/secure/Dashboard.jspa?selectPageId=10104

你也可以查找和审查现有的问题，选择一个感兴趣的问题开始在上面工作： https://jira.hyperledger.org/browse/FAB-5491?filter=10580

可以通过这个链接查看怎么使用JIRA文档：https://wiki.hyperledger.org/community/jira-navigation

Gerrit用来提交PR，管理代码评审和检入代码。所有的代码都可以fork和查看： https://gerrit.hyperledger.org/r/#/admin/projects/

使用Gerrit的指导：https://hyperledger-fabric.readthedocs.io/en/latest/Gerrit/gerrit.html

### Rochet.Chat
可以参加实时聊天 Rocket.Chat（类似Slack），用Linux基金会ID：https://chat.hyperledger.org/home

关于Fabric项目有超过24个频道。 #Fabric 频道用来讨论Fabric项目。可以从这个链接查到这些频道的指南： https://wiki.hyperledger.org/community/chat_channels


## 9. 结论
---
### 访谈
结论 - Alexandra Groetsema
> 恭喜，你已经完成了超级账本Fabric课程的学习.
>
> 希望你已经对超级账本Fabric有了更好的感受，并且有兴趣在你自己的分布式账本方案中使用Fabric。
> 
> 如果你想要更深入的参与开源项目，欢迎才与Rocket.Chat的讨论，贡献代码以及参加社区的聚会。
>
> 敬请期待不久之后的另一个课程，将会更深入的介绍超级账本Fabric。
>
> 我是Alexandra，后会有期！
---
