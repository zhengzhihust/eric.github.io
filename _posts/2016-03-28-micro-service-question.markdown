---
layout: post
title:  "Java 编码规范"
date:   2016-03-28 22:39:00 +0800
categories: jekyll update
group: navigation
author: Eric Zheng
tags: Java
---

### 微服务体系实施过程中遇到的问题

In computing, microservices is a software architecture style in which complex applications are composed of small, independent processes communicating with each other using language-agnostic APIs. These services are small building blocks, highly decoupled and focussed on doing a small task, facilitating a modular approach to system-building. One of concepts which integrates microservices as a software architecture style is dew computing.

Properties of microservices architecture (MSA):

* The services are easy to replace
* Services are organized around capabilities, e.g., user interface front-end, recommendation, logistics, billing, etc.
* Services can be implemented using different programming languages, databases, hardware and software environment, depending on what fits best
* Architectures are symmetrical rather than hierarchical (producer - consumer)

A microservices-based architecture

* lends itself to a continuous delivery software development process
* is distinct from a service-oriented architecture (SOA) in that the latter aims at integrating various (business) applications whereas several microservices belong to one application only

#### 一、微服务的职能划分问题。微服务是干什么的！

稍微简单的业务如order微服务可以很清楚地知道其定位。

但最开始的order微服务在做的过程中，会把握不好OrderIdMap、XDOrderEx在order微服务中的位置。原则上微服务要向上屏蔽业务数据模型细节、只暴露业务模型。所以我们讨论确定目前order微服务内部强依赖OrderIdMap、XDOrderEx，可以把这两个部分作为内部模块（但也会有内部的Service API。以后可能会有进一步拆分需求），对外整体以Order的业务模型和API开放出去。
#### 二、各个微服务间的编码风格差异

服务分层后，各层数据对象命名和职责划分风格问题。

综合服务化工程中，目前的退款服务分层有Facade层的DTO、组件层的optDTO、DAO层的DO。仓储服务化中有引入BO的概念。

微服务中，也涉及到服务分层，Facade层、Component组件层、DAO层。各层的数据对象的规范亟需确定。否则10多个微服务，写出来风格各异，与我们微服务建设的初衷相背离。

#### 二、微服务的开发、测试效率问题

服务多起来后，环境部署和维护的稳定性方面难度将增加。所以增加基于Docker和虚拟机的持续集成持续部署的方式来尝试解决。

#### 参考文章：

* [微服务架构的设计模式](http://www.infoq.com/cn/news/2015/04/micro-service-architecture)

* [使用API网关构建微服务](http://www.infoq.com/cn/articles/construct-micro-service-using-api-gateway)

* [实施微服务，我们需要哪些基础框架？](http://www.infoq.com/cn/articles/basis-frameworkto-implement-micro-service)

* [Anti-Patterns Working with Microservices](http://www.infoq.com/news/2016/03/microservices-anti-patterns)
