# 超级账本入门

## 介绍

本章从整体上介绍超级账本，它是Linux基金会管理的一系列项目，专注于商业区块链技术，本章也会介绍当前（2017年10月）超级账本包含的框架和模块。

## 学习目标

* 能够解释超级账本和permissionless区块链技术的区别
* 讨论超级账本如何利用开放标准和开放管理来支持商业解决方案
* 讨论超级账本框架（Iroha，Sawtooth，Fabric，Indy和Burrow）以及模块（Cello，Explorer和Composer）

## 访谈

超级账本 - Navroop Sahdev

> 超级账本是一个开源项目，用来促进跨行业的区块链技术发展。它是一个在Linux基金会管理下的全球范围内多行业合作的组织。你可以认为超级账本是一个面向市场，数据共享网络，小额现金业务和分布式数字社区的操作系统。
>
> 大家共同的目标时显著降低业务开展的成本和复杂性。超级账本区块链是permissioned区块链，也就是说参与网络的各方通常是经过身份管理模块认证的。根本上来说，超级账本区块链是专门为企业设计的解决方案。
>
> 如果你看一下permissionless的区块链，比如比特币和以太坊，那么可以发现任何人都可以加入网络，也就是说不可避免的会有攻击者进入到网络中。超级账本会降低这样的风险，保证只有参与交易的各方才会进入到网络。
>
> 相对于将交易在全网传播，超级账本会控制只在交易各方的范围内可以查看交易记录。超级账本提供了区块链架构、数据隐私、信息共享、不可更改等特性，使用全栈安全协议，完全针对企业应用设计。

## 超级账本

超级账本是一组开源项目，专注于跨行业的分布式账本技术，由Linux基金会组织，覆盖了技术、金融、银行、供应链、制造业和IOT等领域的领导者。

截至2017年10月，超级账本一共包含8个项目，5个是分布式账本框架。其他3个是支持这些框架的模块。

![](/assets/modular_umbrella.jpg)

[Arnaud Le Hors](https://www.hyperledger.org/blog/2017/09/12/3431)，超级账本技术筹划指导委员会成员，强调指出：

> “这些项目展示了区块链应用技术真正能够达到的广度，远超密码学货币的范畴”  
> 在基于密码学货币的区块链模型之外，超级账本提供了另一种模式，专注于为全球企业开发区块链框架解决方案。超级账本专注于为区块链开发提供一个透明的和协作的方法。

在基于密码学货币的区块链模型之外，超级账本提供了另一种模式，专注于为全球企业开发区块链框架解决方案。超级账本专注于为区块链开发提供一个透明的和协作的方法。

---

### 访谈

超级账本的诞生 - Brian Behlendorf

> 主持人：过去几年超级账本的开发和演进速度之快是恨神奇的。我想知道超级账本是怎么诞生的呢？
>
> Behlendorf：超级账本项目开始是因为几家公司开始关注到比特币领域，这是个密码学货币领域，也是区块链领域。我们来到Linux基金会，然后说“让我们一起做一个项目吧”。Linux基金会15年来，是Linux生态的心脏，把公司和开发者召集在一起试图为通用技术平台的开发进行协调。过去的几年，Linux基金会已经进入到其他领域：软件定义网络、云计算等像这样的东西，以及将Linux生态社区和后面的社区结合在一起。
>
> 很自然的，像IBM或者更年轻的科技公司，像以前没来没有和Linux基金会合作过的JP摩根，或者这个领域的创业公司比如数字资产公司等，当大家开始讨论哪里比较适合作为这个项目的总部时，大家不约而同的认为Linux基金会是最好的选择。
>
> 仍然需要艰苦的努力来找到通往目标的道路。所以在2015年的十二月，Linux基金会整合了大约30家公司，宣布超级账本项目启动。
>
> 在2个月之后的2016年2月，超级账本的第一版软件发布，这就是Fabric。这个项目最初是在IBM内部开发的，现在是社区的一部分了，之后不久是Sawtooth Lake项目。
>
> 不仅是代码，这个社区定期准时的开展面对面的会议，大约是2个月一次。这是因为有太多的东西需要梳理，有太多的知识需要分享。我是在2016年5月加入的。我来的不是最早的，但是因为我看到了这个项目从一开始就是是那么透明，那么迷人。

---

### 超级账本和比特币，以太坊的对比

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

### 超级账本的目标
超级账本已经在开发跨行业的标准方面起到了领导者的作用，位软件协作提供了一个中立的空间。特别是金融服务行业，正在见证着以前的竞争对手现在变合作队友。

新的底层基础或基础设施技术的到来，比如就是区块链，跟以前的因特网一样，需要各个角色之间的协作，以便从新技术中获得最大的利益。如果大家都敝帚自珍，那么技术传播就会非常缓慢。使用新技术的特点就是网络效应，某种新技术使用越广泛，成本就越低廉。

因为转移到分布式账本技术需要大量的成本，那么于此相关的开源软件，社区和生态都大有可为。

![](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/91890fc035a1199d4480f76b6fc743eb/asset-v1:LinuxFoundationX+LFS171x+3T2017+type@asset+block/Hyperledger_Goals.jpg)

### 开放标准
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
#### 访谈
开源的重要性 - Brian Behlendorf
So, you had mentioned before how Hyperledger is different from other consortiums because of its focus on creating an open community, not just open sourcing code.
Can you tell us a little bit about why this is important?
So, open source communities, I believe, live and breathe on not just, you know, reporting bugs or, you know, downloading code,
but, actually, live and breathe on true collaboration, on saying not just, you know, "I have a bug and somebody needs to fix it",
but, instead, saying "Here's how I'd like to solve this", right? What do people think?
"Here is a design for implementing a new feature." What do people think?
And I think the evidence shows that that creates higher quality code. It also creates more long lasting code.
I don't think you have a project like the Linux kernel, which has now been around for 26 years, right, without having a mechanism for a community to develop an institutional memory, right?
Why were certain decisions made? What were some ideas that were proposed and, ultimately, found to either be shot down or not really good enough, right,
so that we don't end up changing things back and forth, right, we don't end up making mistakes that we could have avoided before or learning from previous...
how do we make sure we learn from previous mistakes, right?
And that's only possible in an open source community if, on top of just releasing code, you're also engaging in the creative process itself, and making that public facing.
And I think, really, we found out over 25 years of open source that that's the best way to build trustworthy software.
I believe it leads to higher security software and higher quality software, but really, the question is trust.
This is code that sits... At Hyperledger, this is code that will sit at the heart of these enterprises.
This will be their system of record, right? So, it's essential that we develop this code in a trustworthy way and, while we do all sorts of things to help them trust the data in the system,
ultimately, they have to trust the software, as well.
And what I hope is that, through these processes, they can rest assured that, when they pick up Hyperledger, anything, right, that anything will be software that they can trust.
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
##### 访谈

超级账本项目的软件管理 - Brian Behlendorf
How does the software governance of all of the Hyperledger projects work?
So, in open source projects you do... it's not a free-for-all, right? It's not just everybody throwing in every line of code,
hoping that what, you know, it sticks to the wall, and that everything is fine, right?
There actually is a development process that involves decision-making about what comes in and what doesn't, right?
At the core of each of the projects at Hyperledger is a set of maintainers.
These are individuals who either were with the project when it came in, as initial maintainers, right,
because they had been working on the code before, or were invited in by that initial set of maintainers to become maintainers,
after demonstrating, you know, a history of contributions to the project, alright?
These are now individuals who are trusted.
Once you're in that group, obviously, everything those maintainers do is public.
If they commit to the source code repository, approve a a patch, a commit request, or pull request,
everything they do is public anyway, so there's always that accountability for their actions.
If somebody does something wrong, anybody can always say "I think that's wrong",
and the set of maintainers can sometimes come to a decision to reverse a commit, right?
That's within their power.
But, it's really up to those maintainers to also chart the path forward: what's the roadmap, when will we do the next release, right,
the next minor point released, the next major release.
But, obviously, they do that in a public way.
Sometimes, they'll use a phone call, sometimes, they'll use chat on Rocket Chat, sometimes, they'll use email,
but they know that their job is to be accountable and responsible to the broader development community.
Now, from a Hyperledger perspective, if we leave it up to the projects to really decide the roadmap of what they're trying to solve,
there's a group called the Technical Steering Committee, though,
that is elected by all the contributors across all the projects, not even just the maintainer...
anybody who has contributed a line of code, contributed to the wiki in some substantial way,
they elect a group of eleven developers who form this Technical Steering Committee.
And the TSC is kind of an oversight body.
They make sure that the projects are growing, and that they're healthy.
They review activity in those projects, they also approve new projects when they come in, and they approve the graduation from the Incubator, right?
What they don't get to do is, you know, tell us, tell a project "Hey, you're working on the wrong thing. Go work on this, instead", right?
They really have to depend on this kind of decentralized governance, right, to aim in the right direction.
By and large, we try to make sure though that every project has some sort of relation to distributed ledgers and smart contracts.
Our goal is not to be the GitHub of distributed ledgers and smart contracts projects.
There's already a GitHub for that, alright?
We want these projects to be a curated, you know, coherent portfolio of different projects that might even compete with each other, right?
In many ways, Fabric and Sawtooth and Iroha do overlap, and you can build an implementation of something in all three of those. That's okay, right?
We're going to discover over time how these projects differentiate with each other,
and it's the role of the Technical Steering Committee, and a bunch of other committees we have around identity, and architecture, and white papers, and things,
to try to weave these different efforts together in something that looks coherent, something that makes sense for developers.
---

### 商用区块链
由公链比如比特币和以太坊推广开来的密码学货币区块链现在并不能满足许多组织使用区块链技术的需求，比如金融业务、医疗和政府事务。

超级账本是一个独特的平台，专门为企业应用设计了permissioned分布式账本框架，包括那些很强合规要求的行业。企业用例要求诸如扩展性、吞吐量、内建或者可互操作的身份模块这样的能力，甚至需要对监管者开放所有账本数据的只读权限用于保证合规。后者尤其重要，因为无论怎么创新，都必须在当前的监管框架内操作，还要遵守专门为区块链技术制定的新规则。

---
#### 访谈
为什么商业要选择超级账本 - Brian Behlendorf
And, as a follow-up, why are businesses choosing to use Hyperledger over other distributed ledger technologies?
So, companies, when they decide what open source technologies to use, right, they should evaluate an open source project based on a number of factors,
not just, you know, is the code available, does it run, how mature is it, right, have they released a 1.0, and a 1.1, and a 1.2...
You want to see a kind of a regular stream of these things.
We also believe companies make decisions... I believe companies make decisions about what open source technologies to choose based on the health of the community, right?
And how many other companies are embedding this technology inside of their own solutions, right, inside of other products and services,
how many people out there are using it... that sort of thing.
And so, at Hyperledger, what we're trying to do is not just build a very healthy developer ecosystem around our technologies.
We also do a lot to try to talk with our members and others who are using Fabric, or using Hyperledger Sawtooth, or these other technologies,
to understand where are they using it and what's the value that they're getting from it.
And can we talk about that to the outside world, alright, which is hard.
Sometimes, people don't want to talk about their behind-the-scenes projects, right?
But, when we can talk about the application of that in music licensing, or food supply chain projects, alright,
and start to talk about kind of a higher level impact that these projects can have,
then, that I think gets companies really interested.
But, they don't make the decision to jump in, unless they can see that this is not just a piece of code, that this is a movement.
So, that's really what we're trying to build, and that's why we believe companies can confidently decide to pull down and start using Hyperledger Fabric, Hyperledger Sawtooth, and any of these projects.
And hopefully, become contributors to them, as well.
---

### 超级账本孵化的项目
下面的访谈Brian Behlendorf介绍了超级账本的项目（2017年10月）。包括超级账本框架，本章会深入讨论。包括超级账本模块，下一章会深入讨论。

---
What are the projects currently being incubated by Hyperledger?
So, at Hyperledger, we have a total of eight different projects, actually, as of this week [October 2017].
Because we always have the door open to new projects joining.
And we evaluate those projects on, you know, how do they fit with the portfolio of other projects,
are they mature enough to really understand, you know, where they're going to go and who the developers are around them...
But, every new project that comes in, comes in through something called the Incubator, right?
Because we figured there's a period of time where we need to do a number of things:
we need to make sure that all of the intellectual property associated with that code is actually being contributed correctly,
and, more importantly, we need to make sure that the development team that is coming along with that code understands how to use the tools,
but also understands how to work collaboratively and publicly. Alright?
This is sometimes a new thing for many of these types of projects.
So, Fabric has graduated from the Incubator, to be an ordinary full-fledged project in Hyperledger.
Sawtooth has graduated as well. [Iroha has also graduated from the Incubator.]
The other projects are still working through that process, and the goal is to get every one of the projects through.
We don't try to apply a flamethrower to, like, nudge them through too strongly, right?
But, you know, the hope is that, over time, that they do become that,
and that becomes yet another trademark, a signal to the public that this is now code that they can trust, right?
That this is now something they can build upon.
And so, that's one of these stages. Another one is, for example, releasing as 1.0, which the Fabric community did a few months ago.
So, we have all these signals that we hope to be able to send to the public, and these other projects are still on that path.
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
##### Sawtooth 0.8
##### Fabirc 1.0
##### Indy
##### Burrow 0.16.1

