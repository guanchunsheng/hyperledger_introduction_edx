# 0. 欢迎入坑

## 0.1 写在前面
这篇Hyperledger的教程，源自edX的培训课程，为了方便快速浏览和复习，这里做了翻译和整理。  
对于需要学习的同学，可以在edX培训网站的课程培训部分学习并参加考试，考试合格后还可以付费获得结业证书。  
课程连接：https://courses.edx.org/courses/course-v1:LinuxFoundationX+LFS171x+3T2017/course/

本课程通过Hyperledger项目及其核心框架，介绍商业区块链和分布式账本技术。现在很多行业都对区块链有极大的兴趣。本课程作为入门课程，针对技术人员和业务人员进行了针对性的设计，涵盖企业级区块链技术及相关案例。

Hyperledger是一组由Linux基金会组织管理的开源区块链项目。各行各业都在研究如何使用区块链提升效率，解决跟数据隐私性、安全性、信息共享等业务问题。那么什么是区块链、分布式账本技术，它们又会如何影响全球商业呢？

本课程首先会介绍区块链技术的核心特性，几种Hyperledger框架之间差异。然后从“什么是区块链”开始，讨论什么场景适合使用区块链技术。最后通过具体案例的学习，深入探索企业级Hyperledger区块链框架。

技术人员可以通过本课程的学习，掌握安装Hyperledger Sawtooth和Fabric工程的步骤，并且在这些框架上开发简单的应用。以此理解不同类型的区块链项目，并且根据实际项目需要选择最适合的框架。

业务人员可以通过本课程的学习，掌握区块链的工作方式，区块链是怎么通过降低成本、提升效率（节省时间和简化过程）来为业务做出贡献。可以了解到信息是如何在不同的区块链上产生、保存和共享的。可以掌握评价方法，评估具体业务需要是否适合用区块链方案来解决。

---
**Brian Behlendorf - Hyperledger执行董事**
> 大家好！我是Hyperledger项目的执行董事Brian Behlendorf，这个项目是Linux基金会管理下的一些相关项目一起组成的。
>
> 欢迎来到Hyperledger的第一个培训课程。
>
> Hyperledger是第一个关注企业级区块链应用的系列项目。
>
> 也就是说Hyperledger是封闭（permissioned）的区块链技术。
>
> Hyperledger在各种商业流程中自动执行智能合约，构建了一个安全可信的信息系统，一个能够自动执行商业流程的系统，一个美丽的新世界。
>
> 本课程中，你将学习3个不同的项目，开始直接跟这项技术亲密接触，了解何如通过这些项目构建一个简单的应用。
>
> 本课程适合任何人，从程序员到技术关注者，了解这项技术能够干什么，怎么用，可以用到什么样的业务上。
>
> 本课程是为了从整体上了解这项技术而设计的。
---

## 0.2 课程学习目标

* 能够描述商业区块链和分布式账本技术
* 熟悉当前的Hyperledger项目及其跨行业的使用场景
* 掌握Hyperledger Sawtooth和Fabric框架的安装步骤
* 探索Sawtooth和Fabric框架中的一个简单的用例/应用
* 基于Sawtooth和Fabric构建简单的应用
* 参与并为开源Hyperledger项目做贡献

## 0.3 关于edX课程的使用方法

> 注：略，请参考edX网站关于课程使用方法a的说明

---
**Brian Behlendorf - Hyperledger执行董事**
> **这个课程背后的预期是什么呢？你希望学员从中学到什么？**
>
> 这个课程是为了帮助技术人员从整体上更好的理解Hyperledger的项目，它们如何工作，差异是什么，怎么使用。同时这个课程也是为了那些曾经是技术人员，现在想要了解区块链技术怎么应用于解决商业问题的人。这个课程会帮助他们理解这项技术适合于什么样的用例。
>
> 所以这个课程其实想要提供一个整体视角，介绍区块链是什么，当我们提到商业区块链的时候，我们说的是什么，它跟密码学货币的区别是什么，它们的机制是什么，共识机制是什么，智能合约是什么，诸如此类。
>
> 我们的目标是给更多的学员提供课程，希望能够带领技术人员和开发者入门，了解各项目的更多的信息，成为这个技术的开发者。

## 0.4 Hyperledger

![](/assets/Hyperledger_logo.png)

Hyperledger是Linux基金会从2015年创建和管理的一组开源项目。它的目标是推动跨行业的区块链技术，保证商业伙伴间的可问责、透明和信任。Hyperledger可以让商业网络和交易更有效率。

这些优点在很多行业都颇具价值，比如技术行业、金融、医疗、供应链和汽车行业等。

Hyperledger提供了不同的区块链平台，本课程会介绍其中的3个：Iroha，Sawtooth和Fabric。

想要了解更多关于Hyperledger的信息，请点击[这里](https://www.hyperledger.org/)。

## 0.5 Linux基金会
Linux基金会与世界顶级开发者和公司合作，以解决最困难的技术难题，加速开源技术的开发和商业适配为己任。Linux基金会设定其使命为：通过开源协作，提供专业的复杂问题的初始解决方案，为管理开源项目提供工具，包括安全性最佳实践、管理、运营和生态系统建设，培训以及认证等。

Linux是史上最大的，最广泛的开源软件项目。Linux基金会是Linux创立者Linus Torvalds和维护领导者Greg Kroah-Hartman的大本营，为Linux内核开发提供保护和促进。Linux的成功促进了开源社区的成长，展示了开源商业效能，促成了各个行业中，各个技术之上无数的新项目。

今天Linux基金会的工作已经远远的扩展到了Linux之外，促进了软件技术各个层面的创新。Linux基金会是许多核心开源项目的集团组织，这些项目无处不在：

* 大数据和分析：[ODPi](https://www.odpi.org/)， [R Consortium](https://www.r-consortium.org/)
* 网络：[OpenDaylight](https://www.opendaylight.org/)，[OpenFV](https://www.opnfv.org/)
* 嵌入式：[Dronecode](https://www.dronecode.org/)，[Zephyr](https://www.zephyrproject.org/)
* Web工具：[Js基金会](https://js.foundation/)，[Node.js](https://nodejs.org/en/)
* 云计算：[Cloud基金会](https://www.cloudfoundry.org/)，[Cloud Native Computing基金会](https://www.cncf.io/)，[Open Container Initiative](https://www.opencontainers.org/)
* 汽车：[Automotive Grade Linux](https://www.automotivelinux.org/)
* 安全：[The Core Infrastructure Initiative](https://www.coreinfrastructure.org/)
* 区块链：[Hyperledger](https://www.hyperledger.org/)

### 0.5.1 Linux基金会活动

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

### 0.5.2 Linux基金会提供的培训

* 集中培训
* 在线
* 上门培训
* 活动培训

更多关于具体课程的培训信息：

* [Linux编程和开发培训](https://training.linuxfoundation.org/linux-courses/development-training)
* [企业IT和Linux系统管理课程](https://training.linuxfoundation.org/linux-courses/system-administration-training)
* [开源适应性培训](https://training.linuxfoundation.org/linux-courses/open-source-compliance-courses)


- [**目录**](README.md)
- [**下一章 区块链技术初探**](chapter1_introduction.md)
