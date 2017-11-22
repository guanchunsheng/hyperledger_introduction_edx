# 2. 超级账本入门

## 2.1 介绍

本章从整体上介绍超级账本，它是Linux基金会管理的一系列项目，专注于商业区块链技术，本章也会介绍当前（2017年10月）超级账本包含的框架和模块。

## 2.2 学习目标

* 能够解释超级账本和permissionless区块链技术的区别
* 讨论超级账本如何利用开放标准和开放管理来支持商业解决方案
* 讨论超级账本框架（Iroha，Sawtooth，Fabric，Indy和Burrow）以及模块（Cello，Explorer和Composer）

---
**超级账本 - Navroop Sahdev**
> 超级账本是一个开源项目，用来促进跨行业的区块链技术发展。它是一个在Linux基金会管理下的全球范围内多行业合作的组织。你可以认为超级账本是一个面向市场，数据共享网络，小额现金业务和分布式数字社区的操作系统。
>
> 大家共同的目标时显著降低业务开展的成本和复杂性。超级账本区块链是permissioned区块链，也就是说参与网络的各方通常是经过身份管理模块认证的。根本上来说，超级账本区块链是专门为企业设计的解决方案。
>
> 如果你看一下permissionless的区块链，比如比特币和以太坊，那么可以发现任何人都可以加入网络，也就是说不可避免的会有攻击者进入到网络中。超级账本会降低这样的风险，保证只有参与交易的各方才会进入到网络。
>
> 相对于将交易在全网传播，超级账本会控制只在交易各方的范围内可以查看交易记录。超级账本提供了区块链架构、数据隐私、信息共享、不可更改等特性，使用全栈安全协议，完全针对企业应用设计。
---

## 2.3 超级账本

超级账本是一组开源项目，专注于跨行业的分布式账本技术，由Linux基金会组织，覆盖了技术、金融、银行、供应链、制造业和IOT等领域的领导者。

截至2017年10月，超级账本一共包含8个项目，5个是分布式账本框架。其他3个是支持这些框架的模块。

![](/assets/modular_umbrella.jpg)

[Arnaud Le Hors](https://www.hyperledger.org/blog/2017/09/12/3431)，超级账本技术筹划指导委员会成员，强调指出：

> “这些项目展示了区块链应用技术真正能够达到的广度，远超密码学货币的范畴”  
> 在基于密码学货币的区块链模型之外，超级账本提供了另一种模式，专注于为全球企业开发区块链框架解决方案。超级账本专注于为区块链开发提供一个透明的和协作的方法。

在基于密码学货币的区块链模型之外，超级账本提供了另一种模式，专注于为全球企业开发区块链框架解决方案。超级账本专注于为区块链开发提供一个透明的和协作的方法。

---
**超级账本的诞生 - Brian Behlendorf**
> **过去几年超级账本的开发和演进速度之快是恨神奇的。我想知道超级账本是怎么诞生的呢？**
>
> 超级账本项目开始是因为几家公司开始关注到比特币领域，这是个密码学货币领域，也是区块链领域。我们来到Linux基金会，然后说“让我们一起做一个项目吧”。Linux基金会15年来，是Linux生态的心脏，把公司和开发者召集在一起试图为通用技术平台的开发进行协调。过去的几年，Linux基金会已经进入到其他领域：软件定义网络、云计算等像这样的东西，以及将Linux生态社区和后面的社区结合在一起。
>
> 很自然的，像IBM或者更年轻的科技公司，像以前没来没有和Linux基金会合作过的JP摩根，或者这个领域的创业公司比如数字资产公司等，当大家开始讨论哪里比较适合作为这个项目的总部时，大家不约而同的认为Linux基金会是最好的选择。
>
> 仍然需要艰苦的努力来找到通往目标的道路。所以在2015年的十二月，Linux基金会整合了大约30家公司，宣布超级账本项目启动。
>
> 在2个月之后的2016年2月，超级账本的第一版软件发布，这就是Fabric。这个项目最初是在IBM内部开发的，现在是社区的一部分了，之后不久是Sawtooth Lake项目。
>
> 不仅是代码，这个社区定期准时的开展面对面的会议，大约是2个月一次。这是因为有太多的东西需要梳理，有太多的知识需要分享。我是在2016年5月加入的。我来的不是最早的，但是因为我看到了这个项目从一开始就是是那么透明，那么迷人。
---

### 2.3.1 超级账本和比特币，以太坊的对比

下面的表格展示了超级账本permissioned分布式账本与比特币，以太坊permissionless区块链的区别。如果你在为你的业务考虑使用区块链方案，那么需要特别注意这些元素，根据你自己的用力权衡利弊。

|  | 比特币 | 以太坊 | 超级账本 |
| :--- | :--- | :--- | :--- |
| 基于密码学货币 | 是 | 是 | 否 |
| Permissioned | 否 | 否 | 是（一般来说） |
| 伪匿名 | 是 | 否 | 否 |
| 可审计 | 是 | 是 | 是 |
| 账本永久性 | 是 | 是 | 是 |
| 模块化 | 否 | 否 | 是 |
| 智能合约 | 否 | 是 | 是 |
| 共识协议 | PoW | PoW | 多种 |

* Sawtooth可以配置成permissionless的
* 超级账本的共识算法
  * Fabric的Apache Kafka
  * Sawtooth的PoET
  * Indy的RBFT
  * Burrow的Tendermint
  * Iroha的YAC

更多细节请参考[Hyperledger Architecture, Volume 1](https://www.hyperledger.org/wp-content/uploads/2017/08/HyperLedger_Arch_WG_Paper_1_Consensus.pdf)

### 2.3.2 超级账本的目标
超级账本已经在开发跨行业的标准方面起到了领导者的作用，位软件协作提供了一个中立的空间。特别是金融服务行业，正在见证着以前的竞争对手现在变合作队友。

新的底层基础或基础设施技术的到来，比如就是区块链，跟以前的因特网一样，需要各个角色之间的协作，以便从新技术中获得最大的利益。如果大家都敝帚自珍，那么技术传播就会非常缓慢。使用新技术的特点就是网络效应，某种新技术使用越广泛，成本就越低廉。

因为转移到分布式账本技术需要大量的成本，那么于此相关的开源软件，社区和生态都大有可为。

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/91890fc035a1199d4480f76b6fc743eb/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Hyperledger_Goals.jpg)

### 2.3.3 开放标准
> “只有开源协作的软件开放方式可以保证透明性、持久性、互操作性，从而支持区块链技术进入主流的商业应用领域。这就是超级账本的意义-构建区块链框架和平台的软件开发者的社区”  
>
> [hyperledger.org](https://www.hyperledger.org/about)

如我们在第一章学习的，分布式账本技术缺乏标准是推广它的主要障碍之一。超级账本的主要目的之一就是促进标准化过程，不是仅仅促成它自己的分布式账本，而是提供了一个多标准同时共存的空间：
> “不是提出单一的区块链标准，它鼓励通过社区协作的方式开发区块链技术，使用鼓励开放开发的知识产权，不断采用关键的标准”
>
> [hyperledger-fabric.readthedocs.io](https://hyperledger-fabric.readthedocs.io/en/latest/)

超级账本的目的在于坚持**开放标准**，意思是：
> “通过公开发布的接口和服务进行互操作”
>
> *John Palfreyman*, [ibm.com](https://www.ibm.com/blogs/insights-on-business/government/open-innovation-blockchain-hyperledger/)

---
**开源的重要性 - Brian Behlendorf**
> **你已经提到超级账本跟其他团体的区别是因为它更关注于创建开放社区而不局限于开放源码。那么能不能跟我们说一说为什么这个很重要？**
>
> 我相信，开源社区并不是热衷于报告bug或下载代码，实际上我们真正喜欢的是合作，不是说“我有个bug，谁来修复一下”，而是“这里我想这么改一下”，大家会怎么想？
> 
> “这里有个新特性的设计”。大家会怎么想？
> 
> 我认为有证据表明这样会创造更高质量的代码。也会创造更持久的代码。
> 
> 我认为离开了这样的社区机制所创造的组织传承，是不可能维护像Linux内核这样的有26年历史的项目。
> 
> 为什么要做某项决定？那些开始提出最后因为不够好或者什么原因毙掉了的想法怎么来的？我们总在变来变去，我们总在犯本可以避免的错误。
> 
> 我们怎么确定已经从过去的错误吸取教训了呢？只能是一个开源社区，在那之上发布代码，你也会参与到创作过程，并将之面向公众。
> 
> 我们发现25年来，开源是构建值得信任的软件的最好方式。我相信这也是更安全的软件和更高质量软件的来源。
> 
> 但问题也是有的，那就是信任。这些代码会运行在企业的核心，会是他们的记录系统。所以很重要的一点是我们要用值得信任的方式开发代码，要竭尽所能帮助他们相信系统中的数据。
> 
> 最终，也需要他们的信任，对于我们的软件。
> 
> 我所希望的是，通过这些过程，他们能够确信，当他们使用超级账本的时候，这就是他们可以信赖的软件。
---

#### 开源和开放管理
> “今天多数人已经理解了*开源*的概念。但是人们还不太了解，而我们超级账本和Linux基金会引以为傲的是，*开放管理*”
>
> hyperledger.org

开源软件是自由制作、传播和修改的软件。换句话说，每个人都可以查看代码，使用代码，复制代码，修改代码并且根据开源许可，贡献代码。

开源管理的意思是开源软件项目的技术决策是由社区从一活跃参与者中选举出来的开发者所决定的。这些决策包括加入什么特性，以及什么时候加入等。

请通过以下链接了解超级账本开放管理的细节  
https://hyperledger.org/blog/2017/09/06/abcs-of-open-governance

---
**超级账本项目的软件管理 - Brian Behlendorf**

> **你们是如何对超级账本项目的工作开展软件管理的呢？**
> 
> 在开源项目中，并不是完全随随便便的。并不是每个人都可以随便扔一段代码进来，贴到墙上就万事大吉了。
> 
> 决定需要什么不需要什么，是有开发流程的。在超级账本项目各个项目中都有一些维护者。他们或为项目的初始开发维护者，因为他们以前就为项目工作，或者是初始维护者邀请来作为维护者的，参照他们在项目开发过程中做出的历史贡献。
> 
> 这些人是我们信任的人。一旦你在这个组里，那么显然维护者的工作都是公开的。如果他们向代码仓提交源码、批准一个补丁、提交请求或者拉取请求，他们所作的任何事都是公开的，所以他们的所有行为都是可追责的。
> 
> 如果有人犯了错，每个人都可以说：“我认为这是错的”，然后维护者们可以决定恢复一次提交。这在他们的权利范围内。
> 
> 确实要依赖那些维护者来制定发展路线：路线图是什么样的？下一版什么时候发布？下一个小版本，下一个大版本之类的。但是这也还是公开的。
> 
> 有时他们要打电话，有时他们用Rocket Chat，有时他们用email。但是他们知道他们的工作是可问责的，需要对更广泛的开发社区负责。
> 
> 现在从超级账本的角度，如果我们要决定各个项目解决问题的方向，各自的路线图，那么我们有一个称为技术筹备委员会的组织，由所有项目投票选出，不光是维护者，哪怕是贡献了一行代码，贡献了wiki词条，都可以参与选举，选择谁来组成TSC。
> 
> TSC是一个监督实体。他们要保证项目能够健康成长。他们检查项目的行动，批准新项目的进入，批准孵化器孵化。
> 
> 他们不需要做的是跟项目说“你们做错了。不能那么做，要这么做”。
> 
> 他们需要依赖这样的分布式管理，来瞄准正确的方向。
> 
> 总的来说，我们试图通过这样的方式让每个项目跟分布式账本和职能合约有某种关联。我们的目标不是去做分布式账本和智能合约的Github。已经有Github了。
> 
> 我们希望这些项目是能够相互组合甚至相互竞争的。
> 
> 在很多方面，Fabric和Sawtooth以及Iroha是重叠的，你可以在三个平台都实现类似的应用。那也没问题。
> 
> 我们要发现随着时间的推移，这些项目是如何区分开的。
> 
> TSC，还有其他委员会诸如身份、架构和白皮书等小组的作用是把这些不同的项目糅合在一起，这对开发者来说非常重要。
---

### 2.3.4 商用区块链
由公链比如比特币和以太坊推广开来的密码学货币区块链现在并不能满足许多组织使用区块链技术的需求，比如金融业务、医疗和政府事务。

超级账本是一个独特的平台，专门为企业应用设计了permissioned分布式账本框架，包括那些很强合规要求的行业。企业用例要求诸如扩展性、吞吐量、内建或者可互操作的身份模块这样的能力，甚至需要对监管者开放所有账本数据的只读权限用于保证合规。后者尤其重要，因为无论怎么创新，都必须在当前的监管框架内操作，还要遵守专门为区块链技术制定的新规则。

---
**为什么商业要选择超级账本 - Brian Behlendorf**

> **作为后来者，商业上有什么理由选择超级账本而不是其他分布式区块链技术呢？**
> 
> 公司在采用开源技术的时候，需要考虑若干因素，并不只是能获得代码，还有怎么运行，是否成熟，是否发布了1.0，1.1，1.2等等，你肯定是希望能够看到规律的发展节奏。
> 
> 我们也相信工作在做决定的时候，是基于社区的健康来做决定的。会考虑有多少家公司采用了这个技术，有多少人在使用它之类的。
> 
> 所以超级账本项目中，我们在尝试的不仅是构建非常健康的技术开发者生态，我们还做了很多尝试，与我们的成员和其他使用Fabric、Sawtooth或其他技术的公司，去了解他们把这项技术应用到了哪些领域，他们从中能够获取到什么样的价值。
> 
> 我们跟外界的对话并不容易。有时候人们不想谈他们的幕后项目。但是我们可以谈谈在音乐许可、食品供应链项目等等，我们已经在高一点的层次了解超级账本项目会产生的影响，各家公司都这个都很有兴趣。
> 
> 但是他们还没决定是否参与进来，除非他们能确信这不仅仅只是一段代码，而是一个运动。所以这正是我们想要尝试创建的项目，也是为什么我们能确信很多公司会决定使用Fabric，Sawtooth以及其他项目的原因。
> 
> 希望这些公司也能成为贡献者。
---

### 2.3.5 超级账本孵化的项目
下面的访谈Brian Behlendorf介绍了超级账本的项目（2017年10月）。包括超级账本框架，本章会深入讨论。包括超级账本模块，下一章会深入讨论。

---
> **现在超级账本孵化项目都有什么？**
> 
> 在超级账本到本周为止（2017年10月）我们有8个不同的项目，而且我们对于新项目一直是敞开大门的。
> 
> 我们对于那些项目，都会评估是否符合其他项目的利益，它们是不是足够成熟或者真正知道将要如何发展，在上面工作的都是哪些开发人员等等。
> 
> 每个新项目来的时候，都是通过孵化器进入的。因为我们发现在一段时间内我们必须做若干事情：确保相关的所有知识产权都是正常获得的，更重要的是我们要确认即将进来的开发团队理解如何使用工具，如何公开协同的工作。对于很多这类项目来说，这都是新东西。
> 
> Fabric已经从孵化器孵化出来，已经成为超级账本项目的常规成熟项目。
> 
> Sawtooth也已经孵化出来，Iroha同样孵化出来了。
> 
> 其他项目仍然在孵化中，我们的目标是所有的项目都能够孵化出来。
> 
> 我们不会揠苗助长，我们所希望的是经过时间的磨练，变成一个商标，变成受公众信赖的代码。
> 
> 这就是目前项目的情况。几个月前Fabric发布了1.0版本，我们希望通过这样的信号告诉公众，我们其他的项目也都在路上了。
---

#### 超级账本框架组件
超级账本商业区块链框架用于构建企业区块链，用于组织间区块链的共识。这跟比特币和以太坊那样的公链不同。超级账本框架包括：
- 只能添加的分布式**账本**
- 对账本修改的**共识算法**
- 通过permissioned接入实现的的交易**隐私**
- 处理交易请求使用的**智能合约**

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/0747265232da64643d21679294cbbe19/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Components_of_blockchain.jpg)

现在让我们开始探索超级账本的5个框架（2017年10月）！

##### Iroha v0.95
[超级账本Iroha](https://hyperledger.org/projects/iroha)是由Soramitsu, Hitachi, NTT Data, 和Colu贡献的框架。它的设计目的是能够简便的构造需要分布式账本技术的基础设施。Iroha强调移动应用开发，具有安卓和IOS的客户端库，这是跟其他超级账本框架的区别。Iroha受到Fabric的启发，尝试提供C++开发环境作为Fabric和Sawtooth项目的补充。

总之，Iroha的特点是简单的构建，现代的，域驱动的C++设计，共识算法采用[YAC](https://www.overleaf.com/read/wzhwjzbjvrzn#/40115559/)。


##### Sawtooth 0.8
[超级账本Sawtooth](https://www.hyperledger.org/projects/sawtooth)由Intel贡献，是一个使用模块平台构建、部署和运行的分布式账本区块链框架。

用Sawtooth构建的分布式账本方案可以使用多种共识算法，根据网络规模决定。默认使用时间流逝证明PoET共识算法，提供了比特币区块链技术的扩展性，但是去掉了高昂的能源消耗。PoET允许验证者节点规模的扩展性。Sawtooth的设计目的是多变性，既支持Permissioned，也支持permissionless。

---
**超级账本Sawtooth介绍**
> Meet Rich: he owns a popular seafood restaurant in Boston, Massachusetts.
> Rich strives to serve only the freshest, highest quality fish to his patrons,
> but he often has difficulty knowing exactly where it was caught, how it got to his restaurant, or if it's even the right species of fish.
> From ocean to table, the fish supply chain is difficult to track, and usually follows this pattern:
> the fish is caught by a commercial fisherman in the ocean,
> the fisherman then sells the fish to a market, processor, or broker,
> a distributor then transports the fish to a restaurant or grocery store for a consumer to purchase.
> Rich typically only buys from one or two fish distributors that he has built a foundation of trust and personal history with.
> He'd like to expand his menu by adding some different kinds of seafood from other distributors,
> but he worries about integrity, and for good reason.
> He recently read a study by Oceana, which showed that 33% of fish purchased from retail outlets is incorrectly labeled,
> and that illegal fishing represents losses between 10 to 23 billion dollars worldwide.
> Rich knows that mislabeled and illegally sourced fish could hurt his customers,
> his restaurant's reputation, and the environment of our planet.
> Fortunately for Rich and others in the seafood industry,
> Sawtooth Lake blockchain technology can provide an immutable record of the provenance and lineage of various goods, like fish.
> In combination with Internet of Things-enabled sensors, Sawtooth Lake can manage the ownership and journey of fish from ocean to table.
> Sensors can be attached to fish the moment they are harvested,
> immediately and continuously recording data such as the location and temperature of the fish.
> Now, Rich can validate when and where his fish was caught, and that it was stored properly on ice while it was transported.
> The Sawtooth Lake platform can also manage the Chain of Custody fish,
> enabling ownership to be transferred and traded on the blockchain according to smart contracts.
> With Sawtooth Lake as a traceability blockchain, Rich can easily see which fishermen meet his quality standards and feels comfortable doing business with new tradesmen.
> When he serves up a new special to a cherished customer, he can confidently and accurately assure her
> it's the type of fish she ordered, it was stored at safe temperatures, when and where it was caught, how long it took to get to her plate, and the fish's name... just kidding.
> Sawtooth Lake blockchain technology can be used for a wide variety of applications,
> from capital markets to international trade.
> The Internet of Things ties the physical world to the digital world,
> with Sawtooth Lake recording the generated data in a way that all parties can trust its accuracy and completeness.
> This is extremely useful for tracking perishable goods, like fish.
> These sensors can track many key parameters, such as location, temperature, humidity, motion, shock, and tilt.
> This technology could be added to any package or sensitive good you're entrusting to other parties.
> The blockchain will ensure the data is secure and tamper-proof, so that you know what you're getting, and that you get what you pay for.
> Sawtooth Lake creates a digital platform enabling physical traceability in a trustless world.

**Sawtooth的特点**
Dan Middleton
> **Sawtooth特点**
> 我认为我们是唯一一个提供了分布式状态一致性的项目。
> 
> 我的意思是你可以信任系统中每个节点，对信息都有相同的理解，网络中的机器并不因为计算能力的区别而有不同，他们逐渐修改他们的数据库，而不需要与其他节点真的达成什么一直。
> 
> Sawtooth的其他特点还有交易处理接口部分，我们现在恩那个狗为任何种类的交易逻辑创建适配器。
> 
> 最近我们与Burrow的集成就很好的证明了这一点。现在你可以运行任何种类的EVM代码，比如Solidity，可以编译并在Sawtooth上面运行。
> 
> 所以在企业环境，当你考虑以太坊上创建的Solidity代码应该放在哪里运行的时候，Sawtooth和Burrow就是答案。

**Sawtooth的用例特点**
Dan Middleton
> Sawtooth的特点有好几个，我现在想到的一个是存在性或者供应链用例，就是那种网络会随着时间成长的。
> 
> 我们很多人开始使用区块链网络的，很多公司开始关注区块链网络的原因，是因为我们认为这是个长期发展和持续的技术。
> 
> Sawtooth的设计就考虑到网络规模的成长。你可以即时改变共识算法。我认为这是一个Sawtooth和其他账本相区别的独一无二的特点。
> 
> 你可以提交一个交易，然后在你的网络中按照新的共识来接受这个交易，然后你的网络比如说从PBFT共识转换成PoET或者某种随机leader选举共识算法。
> 
> 它让你可以有几十，几百或者几千个节点部署在网络中，在以年记运行的网络中，你真的无法战胜存在性、集成性保证和网络的灵活性。
---

##### Fabirc 1.0
[超级账本Fabric](https://www.hyperledger.org/projects/fabric)是代码库的第一个提案，是由Digital Asset Holdings的前期工作，Blockstream的libconsensus和IBM的OpenBlockchain合并组成的。Fabric提供了一个模块化架构，允许共识算法和成员管理服务按照即插即用的组件来使用。Fabirc革命性的允许实体执行机密交易，而不需要向中心机构传送信息。这是通过网路中运行的不同通道，以及不同节点功能的分离而实现的。最后，需要注意的是与比特币不同，比特币是公链，Fabric支持的是permissioned部署。

> “如果你有一个很大的区块链网络，如果你想要只与特定的对象分享数据，那么可以与这些参与者共同创建一个私密通道。这是Fabric最有特点的地方”
>
> Brian Behlendorf， 超级账本执行理事，Linux基金会

---
**Fabric的特点**
Chris Ferris
> 超级账本Fabric的主要特定是它是一个permissioned的区块链分布式账本技术。
>
> 它跟比特币和以太坊的实现是不同的，后者本质上公开的，permissionless网络，每个人都可以匿名加入。
---

##### Indy
[超级账本Indy](https://www.hyperledger.org/projects)是专门为了分布式身份（identity）设计的。它的目标是通过一些列的：
> “独立于任何特定账本的分布式身份规范和成品，在所有支持它们的DLT上提供互操作性”

由Sovrin基金会贡献，超级账本Indy允许个人管理和控制他们的数字身份。并非是用商业存储大量的个人数据，而是存储指向身份的指针。一旦公司验证通过了其他人的身份，就可以扔掉了。

> “身份有毒资产，是有可能向组织提出赔偿的。”
>
> Brian Behlendorf

确实，从2013年以来，已经丢失和被盗了90亿条数据记录。更可怕的是其中只有4%是加密的（这就是“安全漏洞”）。可以在这个链接看到细节：http://breachlevelindex.com/。

超级账本Indy的核心原则是她的“设计达成隐私”方式。考虑到DLT的数据永久性，更重要的是要极其谨慎地处理数字身份问题，要以人为本。

> “Indy让用户鉴权身份，基于的是他们愿意保存和分享这些身份。这可以减少商业中包含的风险责任数量，因为数据还是用户自己保管并按照你信任的方式重新呈现出来，可以验证曾今刚说过的话确实存在，可以让商业伙伴相互信任”
>
> Nathan George, Indy维护者

---
**Indy介绍**
Nathan George
> The Hyperledger consortium has many different projects that focus on different aspects of how ledgers can work and what use cases they can be applied for.
> Hyperledger Indy is a distributed ledger purpose-built for doing distributed identity,
> and what that means is, it allows you to have a route of trust to manage the keys, schemas, proofs, and other information that you need to,
> in order to enable trusted peer interactions between different identities, as stored on a Hyperledger Indy blockchain instance.
> So, if you have an identity it belongs to you, and only you, and no one can pull the plug on you.
> And you can use that identity to manifest different correlatable pieces of data between you and other identities you want to interact with,
> without leaking private information or disclosing information that you don't want shared across all those different aspects of who you are.
> And when you control your identity, it makes it so that you are also a party to the kinds of data sharing, claims, and proofs that can be made about you,
> as information is shared across all the different interactions you might do online.
> One of the main use cases of Hyperledger Indy is to create a global public utility for identity that's being created by the Sovrin Foundation,
> which allows us to take these identifiers, and to anchor them on a public ledger,
> so that different pieces of truth or pieces of information can be trusted and shared with other people across the world,
> and they can be trusted and validated, so that self-attested data can be shared, as well as third-party attestations can be shared across all kinds of interactions and across all kinds of data silos,
> and what this makes possible is the ability to no longer have to function as an identity provider as a business,
> but to rather let users authenticate based on the attributes that they're willing to store and share themselves,
> which allows for GDPR-compliant use cases, where you're expressing consent on behalf of the user, and also reducing the amount of liability contained within a business,
> because the data can be kept with the user and presented to you again in a way that you can trust and validate,
> that what has been said, really was said, and is trusted by the other parties you do business with.
---

##### Burrow 0.16.1
超级账本Burrow在2014年12月发布，正式名称是eris-db。现在正在孵化的Burrow是一个permissionable智能合约机，提供了一个模块化区块链客户端，内建permissioned智能合约解释器，是以太坊虚拟机（EVM）规范的一部分。是唯一的设定为Apache许可的EVM实现。

主要组件：
- **Gateway** ：为系统集成和用户接口提供接口
- **智能合约应用引擎** ：促进集成复杂的业务逻辑
- **共识引擎** ：由两个目的，在节点之间维护网络栈；排序交易
- **应用区块链接口（ABCI）** ：为共识引擎和智能合约引擎的互联提供接口规范

通过以下链接获取更多信息：  
https://monax.io/platform/db/

### 2.3.6 超级账本模块
前面介绍的超级账本框架是用来构建区块链和分布式账本的。超级账本模块，是部署和维护区块链，检查账本数据的辅助软件，以及设计，原型和扩展区块链网络的工具。

#### Cello
Cello为满足想要部署区块链即服务（Blockchain-as-a-Service，BaaS）的商业区求，提供了一个工具套件。特别是为了精益企业和小公司，如果想要减少所需的成本来创建、管理和终止区块链，超级账本Cello允许区块链部署到云端。维护人员可以通过dashboard创建和管理区块链，用户（一般是chaincode开发者）可以立即获取到一个区块链实例。

作为超级账本的模块，*“Celo的目标是为区块链生态带来按需的'as-a-service'部署模型”*，以此帮助今后的超级账本框架的开发和部署。Cello首先由IBM贡献，由Soramitsu, Huawei, 和Intel赞助。

应用开发者和系统管理员可以使用Cello提供和维护超级账本网络。比如，你可以创建一组分布式账本网络，在虚拟云或称为“容器集群”上，然后用可配置的dashboard管理和监控那些网络。此外，你可以建立一个BaaS平台。

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/a962f1ae98ce3d4796f6d9f315b49572/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/cello.jpg)

#### Explorer
[超级账本浏览器](https://www.hyperledger.org/projects/explorer)是一个区块链可视化操作工具。是第一个permissioned账本的区块链浏览器，让任何人都可以浏览分超级账本成员内部创建的布式账本项目，不会破坏它们的隐私。这个项目是由DTCC，ntel和IBM贡献的。

浏览器设计有用户友好的web应用，可以查看，调用，部署或查询：
- 区块
- 交易及相关数据
- 网络信息（名称，状态，节点列表）
- 智能合约（chaincode和交易族）
- 存储在账本中的其他信息

能够对数据进行可视化是非常重要的，这样才能从中开发出商业价值。超级账本浏览器提供了关键功能。核心组件包括web server，web UI，web socket，数据库和安全仓，以及一个区块链实现。

#### Composer
[超级账本Composer](https://www.hyperledger.org/projects/composer)为构建区块链商业网络提供了一组工具：
- 为你的网络建模
- 与区块链网络交互的REST API
- 生成一个Angular应用框架

由Javascript构建，Composer提供了一组易用的组件，开发者可以很快学习和实现。这个项目是由Oxchains和IBM。

Composer创造了一个建模语言，语序你定义资产，参与者和交易，用商业词汇组成你的商业网络。慈爱，交易逻辑是开发者用Javascript写的。这个简单的接口允许业务人员和技术人员共同工作，定义他们自己的商业网络。

Composer的[好处](https://www.hyperledger.org/wp-content/uploads/2017/05/Hyperledger-Composer-Overview.pdf)是：
- **快速创建区块链应用**，减少从零开始构建区块链应用的大量工作
- **降低风险**，完善的测试、高效的设计，让业务和技术分析者能够对其理解
- **更大的灵活性**， 高层抽象的灵活性让它很容易迭代

可以在[这里](https://www.youtube.com/watch?v=cNvOQp8r0xo)看到更多介绍。

---
**Composer**
Simon Stone 和 Kathryn Harrison
> Composer包含Javascript开发者很熟悉的组件，不管你是前端开发搞UI的，还是后端开发搞服务或者代码的。JS开发者可以很熟悉和简单的使用它来构建区块链应用。
> 
> 我们有数据建模语言，允许你快速描述你的区块链商业网络。可以使用标准JS代码实现你的业务逻辑。我们有基于web的playground来学习Composer的概念，做试验，查看我们的示例业务网络，并且扩展他们。
> 
> 我们有一些客户端库用于构建JS应用，可以插入到你喜欢的编辑器中，Atom或者VS Code。我们有一些命令行工具作为管理系统，创建脚本在区块链中自动化执行任务。
> 
> 我们还创建了应用骨架。所以一旦你花费一些时间对你的区块链业务网络，资产模型和参与方等建好摸，那么几分钟内我们就会建好UI。
> 
> 所有需要做的就是运行一个命令，回答一些简单的问题，然后就可以得到一个全功能的UI app，你可以在这个基础上按照你的用户经验进行裁剪。
> 
> 最后，我们很热衷API的，我们真的想要让你的区块链应用很容易的使用RESTful API，在客户端应用中，或者集成到你的现有的系统中。
> 
> 我么还是提供了简单的命令，你可以运行它，把它连接到你自己的区块链业务网络，然后你可以针对REST API进行构建。
> 
> Composer是让区块链上创建应用得到极大简化的开发工具。如果你思考一下区块链世界，有多个组织需要达成共识，什么类型的参与者，资产和交易是需要纳入进来的。
> 
> 那么Composer允许开发者非常快速和简单的建模出这个商业用例的关键组件，业务和技术团队都能够对齐，创建让应用开发非常简单的API。
> 
> 不仅是为区块链创建应用是很容易的，同时也是让具有Js技能的前端和全栈开发者能够进入区块链开发者行列。
---

### 2.3.7 问答
---
**开发者为什么对开源软件感兴趣**
> So, many of the students taking this course are more familiar with working with Devs within the same room.
> But why do you think developers would be excited in becoming involved with open source projects, such as Hyperledger?
> Well, open source software represents, generally, the largest software development classroom ever, right?
> When I was learning about software development as an undergraduate at the University of California at Berkeley,
> I was taking classes, you know, I was learning kind of the formal bits about, you know, data structures and algorithms,
> but the biggest education came from sitting on development mailing lists around the standards around HTTP and HTML,
> and, eventually, the software development mailing lists and open source communities for the early days of the web.
> And seeing, really, how software gets built, right, what are the trade-offs, what are the negotiations, the back-and-forths, the messiness of software development,
> which doesn't look that different than, say, how Congress, you know, works on the bills sometimes, right?
> Sometimes, it can be, you know, not very pretty, but you realize that software engineering is as much a technical pursuit, as it is a social pursuit.
> And, in open source projects, I think we've figured out how do we have technical differences of opinion and work through them,
> how do we create the best software, the software with the greatest longevity, right? why should we document our code...
> Well, it's because we don't want to have to answer silly questions from the next person trying to understand what we wrote, right?
> So, all of this is a really great education, I think, in understanding how to write higher quality code, whether that ends up being open source code or not...
> That's one reason, I think, for developers to participate in open source projects, whether at Hyperledger or any place else.
> The other is that open source projects are a really good way for you, as a software engineer, to understand what are the kinds of companies I want to work for, right?
> The ones that are actively involved in open source projects, right?
> How do I get to know people at those companies, right? And also, make them aware of my own skills, right?
> And start to develop my own public history of my contributions.
> These days, if you're a software engineer, if you're a software engineer and you're applying to any place interesting, they're going to look at your GitHub repository, right?
> They're going to want to know about your history of contributions to open source projects as a way to evaluate your skills,
> and not just technical, but also your communication skills, your collaboration skills...
> So, all of that means working on open source projects can be tremendously beneficial to your own ongoing education, as well as your ability to build your career.

**超级账本与Apache**
> While at Apache, you were able to successfully build an open software developer community.
> What are some similarities and differences between Hyperledger and Apache in its evolution as an open source project?
> Right... Well, on the Apache web server project, which later became the Apache Software Foundation,
> a lot of what happened was due to luck, was due to inheriting a tradition that had been established since the beginning of the internet,
> of technologists working together on common standards and common code.
> And so, there was kind of a DNA that was built-in and, you know, the web was not that complicated at that point in time, either.
> These standards were pretty simple, right?
> So, we just kind of did what seemed natural. We started a mailing list.
> We started an issue tracking system, a version control system, and we just started working together, very ad hoc, very informally.
> And we became a non-profit about three years after we got started, when we realized we wanted a little more formalism, and a little more protection for the developers.
> But it was all very informal, ad hoc, and many developers took on roles beyond development, such as marketing, such as legal, such as accounting, right?
> And so, Apache's culture has been very much almost like a guild, right? Almost like a user community... a very community-driven kind of thing,
> where there's this validity that comes from that grassroots-kind-of-sense, right?
> Which is great for the kinds of projects that Apache was interested in.
> And Apache has become a home for over 300 different individual software development projects, beyond the web server, right?
> But it has always grown by happenstance, and it's always been open to whatever technology project wanted to come in.
> So, there are some upsides and downsides to that all, right?
> One of the downsides is, by not having a full time staff, it's hard for Apache to really take advantage of all the opportunities out there, to get the word out about who they are.
> It means that developers, you know, have to do these non-developer activities, right?
> And it's hard to do things like police your trademark usage, right, when people are working as volunteers.
> So, The Linux Foundation took a slightly different approach.
> The Linux Foundation said "We don't..." You know, just like with Apache, "we don't want to write software, we don't want to have to pay all the software developers",
> because there's actually a lot to the validity that comes from all of the software development happening by volunteers,
> but there's a lot of non-software development things: marketing, legal, PR, and even the hosting of meetings, the hosting of phone calls,
> all of the logistics of helping these communities operate that perhaps can be taken on, right, by a full-time staff.
> Now, how do you pay that staff, right? You could ask for, you know, cutting charity contributions, right?
> But, a better, more scalable approach is to ask companies to join as corporate members of an organization, right?
> And, they don't get any special privileges on the code, right?
> Well, they don't get their patches in by default, they don't get to veto anyone else's work.
> If you're a developer from IBM, there's no special status.
> You have to still prove your worth, and to which, you would, you know, and be seen as a peer to, say, a 15 year old kid in Romania, who also is really eager to work on distributed ledgers and smart contract systems.
> And so, but what members do get is some help in marketing their activities, building on top of Hyperledger projects, ways to participate at the events that we do.
> When a journalist calls us and asks us for a story about somebody doing something interesting with Hyperledger, we've got a set of members that we... whose stories we can draw from, right?
> So, this balancing act of, you know... as I mentioned, there's a developer community, and there's a commercial community, a corporate community...
> That's the balancing act... that The Linux Foundation has really developed a process for, a template for, and a real science around, and that's what we're bringing to bear on the Hyperledger project.

**Fabric Sawtooth和Iroha的关键特性**
> Could you highlight a key feature for each of the three Hyperledger frameworks: Hyperledger Fabric, Sawtooth, and Iroha?
> So, Fabric, is one of the most mature technology projects in Hyperledger.
> It was the first to hit a 1.0 release, and that release, the architecture, the version 1 architecture reflects a tremendous amount of trial, proof of concept and pilot trial, of Fabric,
> with a lot of different companies out there.
> And they discovered things like, for scalability reasons, you wanted to separate out the ordering service into a separate set of nodes.
> Or, you might want to be able to support subsets of exchanges, so that is why there is a feature called private channels, right?
> And so, those are some of the distinctive parts of Fabric.
> For Sawtooth, there is a number of distinctive features, but the two that stand out are a consensus mechanism called Proof of Elapsed Time,
> which takes advantage of some special features on Intel's chips, and then, an approach to smart contracts called transaction families,
> where you essentially predefine a set of acceptable templates for smart contracts that, then, the rest of the network can use.
> And that is a safer approach to doing smart contracts than a fully programmable, general purpose programming language.
> Finally, Iroha, Hyperledger Iroha, the really distinctive thing about that is it's written in C++, it's very tiny and tightly coded,
> and it's got a terrific set of mobile libraries, as well.
> These are all slight differences.
> Our hope is that, over time, as the projects evolve and mature, and all of them get to a 1.0,
> that we find ways to bring them even closer together, and make them more compatible and modular and find ways for them to work with each other.

**超级账本框架之间的互操作性**
> **你认为将有助于促进这三个不同的超级账本框架之间互操作性的会是什么？**
> 我们有一些事情可以去做来帮助超级账本项目之间的互操作。
> 
> 一个是所有项目的整体架构。我们是基于DLT层，基于智能合约层的技术，其中包含身份部分。然后我们会把这些项目放到一起审视，“好吧，我们可以把Fabric放在这个下面么？我们可以把Burrow放在那个上面么？”
> 
> 当新人来到项目里边，或者接受像本课程一样的培训时，也许我们开始把Fabric的分布式账本部分和智能合约部分分离开，放到不同的组件里。然后他们开始根据自己的目的选择自己想要的组件开始创建区块。
> 
> 就是这样逐渐进行了模块化，在不同的层级定义标准API。因此我们可以得到一个理想的结构，你可以从Burrow里边拿到以太坊虚拟机，可以在Fabric上面运行这个EVM，还可以在Sawtooth上面运行这个EVM。
> 
> 事实上Sawtooth和Burrow社区真的开始这么做了，现在你已经可以在Sawtooth上运行以太坊智能合约了，这真是太酷了。
> 
> 所以，我认为我们会看到更多这样的活动。
---

