# 欢迎入坑

本课程通过介绍超级账本（Hyperledger）项目及其核心框架，来探索商业区块链和分布式账本技术的能力。区块链正在很多行业迅速获取了大量的关注。本课程作为入门课程，经过仔细的设计，对技术人员和业务人员都适合学习。本课程会分析企业级的区块链技术及相关的若干用例。

超级账本是一系列开源的区块链项目，由Linux基金会组织管理。各行各业均在研究如何使用区块链提升效率，解决跟数据隐私性、安全性、信息共享等相关的商业问题。那么什么是区块链、分布式账本技术，它们会如何影响全球商业呢？

本课程囊括了区块链技术的核心特性，以及几种超级账本区块链框架的差异。我们会从“什么是区块链”开始，然后基于各位面对的业务讨论什么场景适合区块链技术。之后我们会通过具体实例的学习，深入探索企业级超级账本区块链框架。

技术人员可以通过本课程的学习，掌握安装超级账本Sawtooth和Fabric工程的步骤，并且在这些框架上开发简单的应用。以此理解不同类型的区块链项目，并且根据实际项目需要选择最适合的框架。

业务人员可以通过本课程的学习，掌握区块链的工作方式，以及区块链是怎么通过降低成本、提升效率（节省时间和简化过程）来为具体业务做出贡献的。可以了解到信息是如何在不同的区块链上产生、保存和共享的。可以掌握评价方法，评估具体业务需要是否适合用区块链方案来解决。

## 人物访谈

**Brian Behlendorf - 超级账本执行董事**

> 大家好！我是超级账本项目的执行董事Brian Behlendorf，这个项目是Linux基金会管理下的一些相关项目一起组成的。
>
> 欢迎来到超级账本的第一个培训课程。
>
> 超级账本是第一个关注企业级区块链应用的系列项目。
>
> 也就是说超级账本是需要准入（permission）的区块链技术。
>
> 超级账本在各种商业流程中自动的执行智能合约，构建了一个安全的，可信任的信息系统，一个能够自动执行商业流程的系统，一个让人激动的新世界。
>
> 本课程，你将学习3个不同的项目，你将开始直接跟这项技术亲密接触，而且可以看到通过这些主要的项目，一个简单的应用是如何构建的。
>
> 本课程适合所有人，从纯码农到想要了解这项技术的人，了解这项技术能够做什么，如何应用到什么样的业务上。
>
> 本课程是为了从很广的角度了解这项技术而设计的。

## 课程学习目标

* 能够描述出商业区块链和分布式账本技术
* 熟悉当前的超级账本项目及其跨行业的使用场景
* 掌握超级账本Sawtooth和Fabric框架的安装步骤
* 探索Sawtooth和Fabric框架中的一个简单的用例/应用
* 基于Sawtooth和Fabric构建简单的应用
* 参与并为开源超级账本项目做贡献

## 关于edX课程的使用方法

> 译作者：这里略过，请参考edX课程中的说明，与本课程内容无关。

## 人物访谈

**Brian Behlendorf - 超级账本执行董事**

> 主持人：这个课程背后的预期是什么呢？你希望学员从中学到什么？
>
> Behlendorf：这个课程是为了技术人员能从整体上更好的理解超级账本项目中不同项目是如何工作的，理解它们的差异，开始了解怎么使用它们。同时这个课程也是为了那些曾经是技术人员，现在有更多想法的人，想要花点时间思考下区块链技术的商业应用问题的人。这个课程会帮助他们理解这项技术适合于什么样的用例。
>
> 所以这个课程确实想要提供一个整体视角，介绍区块链是什么，当我们提到商业区块链的时候，我们说的是什么，它跟密码学货币的区别是什么，它们的机制是什么，共识机制是什么，智能合约是什么，诸如此类。
>
> 我们的目标是给更广泛的学员提供课程，希望能够将那些技术人员和开发者领进门，了解不同项目的更多信息，成为这个技术的开发者。

## 超级账本

![](/assets/Hyperledger_logo.png)

超级账本是Linux基金会从2015年创建和管理的一揽子的开源项目。它的目标是推动和促进跨行业的区块链技术，保证商业伙伴间的可问责、透明和信任。以此，超级账本可以让商业网络和交易更有效率。

这些优点在很多行业都颇具价值，比如技术行业、金融、医疗、供应链和汽车行业等。

超级账本提供了不同的区块链平台，本课程会介绍其中的3个：Iroha，Sawtooth和Fabric。

想要了解更多关于超级账本的信息，请点击[这里](https://www.hyperledger.org/)。

## Linux基金会

> 译作者：Linux基金会的信息跟超级账本关系不大，但是作为知识性的内容，对程序员还有挺有意义，所以保留

Linux基金会与世界顶级开发者和公司合作，以解决最困难的技术难题，加速开源技术的开发和商业适配。Linux基金会设定其使命为：通过开源协作，提供专业的复杂问题的最开始的解决方案，为控制开源项目提供工具，包括安全性最佳实践、管理、运营和生态系统开发，培训和认证等。

Linux是史上最大的，最广泛的开源软件项目。Linux基金会是Linux创立者Linus Torvalds和维护领导者Greg Kroah-Hartman的大本营，为Linux内核开发提供保护和促进。Linux的成功促进了开源社区的成长，展示了开源商业效能，激发了各行各业各种技术之上数不尽的新项目。

今天Linux基金会的工作已经远远的扩展到了Linux之外，促进了软件技术各个层面的创新。Linux基金会是许多关键开源项目的一揽子组织，这些项目无处不在：

* 大数据和分析：[ODPi](https://www.odpi.org/)， [R Consortium](https://www.r-consortium.org/)
* 网络：[OpenDaylight](https://www.opendaylight.org/)，[OpenFV](https://www.opnfv.org/)
* 嵌入式：[Dronecode](https://www.dronecode.org/)，[Zephyr](https://www.zephyrproject.org/)
* Web工具：[Js基金会](https://js.foundation/)，[Node.js](https://nodejs.org/en/)
* 云计算：[Cloud基金会](https://www.cloudfoundry.org/)，[Cloud Native Computing基金会](https://www.cncf.io/)，[Open Container Initiative](https://www.opencontainers.org/)
* 汽车：[Automotive Grade Linux](https://www.automotivelinux.org/)
* 安全：[The Core Infrastructure Initiative](https://www.coreinfrastructure.org/)
* 区块链：[Hyperledger](https://www.hyperledger.org/)

### Linux基金会活动

每年Linux基金会都会组织更多的活动，包括：

* Open Source Summit North America, Europe, Japan, and China
* MesosCon North America, Europe, China
* Embedded Linux Conference/ OpenIoT Summit North America, Europe
* Open Source Leadership Summit
* Automotive Linux Summit
* Apache: Big Data North America & ApacheCon
* KVM Forum
* Linux Storage Filesystem & Memory Management Summit
* Vault
* Open Networking Summit

### Linux基金会提供的培训

* 集中培训
* 在线
* 上门培训
* 活动培训

更多关于具体课程的培训信息：

* [Linux编程和开发培训](https://training.linuxfoundation.org/linux-courses/development-training)
* [企业IT和Linux系统管理课程](https://training.linuxfoundation.org/linux-courses/system-administration-training)
* [开源适配性培训](https://training.linuxfoundation.org/linux-courses/open-source-compliance-courses)



