---
title: 'reading-protocol:amqp0-9-1'
date: 2018-05-07 08:37:05
tags: AMQP
categories: 
  - 学习
  - 协议
---
{% asset_img amqp.jpg %}   

本篇主要的内容是: 解读AMQP协议，协议当前版本是1.0.0，本篇版本的是0.9.1。

<!-- more -->

## 概述

写本篇的原因是：在平时的工作中，经常会有消息中间件的使用，其中涉及到一种在传统金融领域比较流行的一款消息中间件：RabbitMq。这款消息中间件便基于AMQP开发的。在学习了解过改软件的基本使用后，萌生对其实现的协议--AMQP协议一探究竟的想法。本篇从[AMQP官网](http://www.amqp.org/)下载了其0.9.1版本的pdf文档，下面，我将从该文档开始该协议的学习。 

## 主要内容

文档前面的引言之类的，本篇就不在累述了。直接从文档第二部分开始。

### General Architecture 总体结构

#### AMQ Model Architecture

##### Main Entities

{% asset_img AMQ-model.jpg %}  

我们可以总结一下，一个中间件服务应该是什么样：
> 它是一个可以接收消息，并且主要完成以下两件事情的数据服务：
> 
> * 根据其依赖的不同的协议，将消息路由到不同的消费者；
> * 当消费者消费能力不足，或者比消费速度比较慢时，它可以将消息缓存到内存或者磁盘。

在预AMQP服务器中，这些任务由实现特定类型路由和缓冲的单块引擎完成。AMQ模型采用较小的模块块的方法，可以以更多样化和更健壮的方式组合起来。AMQ模型首先将这些任务划分为两个不同的角色：

* **Exchange**，**它负责接收生产者产生的消息**，并且路由到消费队列--Message Queue。
* **Message Queue**，它负责保存消息并将消息转发给消费者应用。

在**Exchange**和**Message Queue**之间还有一个比较清晰的接口，叫做**binding**，这个稍后讨论。AMQP提供了运行时可编译语义，只要通过以下两个方面来提现：

* 具有可以通过该协议创建各种类型的exchange和massage queue的能力；
* 具有可以通过该协议将exchange和message queue绑定在一起，来创建任何需要的消息处理系统的能力。

###### The Message Queue

消息队列。

#### AMQP Command Architecture

#### AMQP Transport Architecture

#### AMQP Client Architecture

### Functional Specification 功能规格

#### Server Functional Specification

#### AMQP Command Specification

### Technical Specifications 技术规格

#### IANA Assigned Port Number

#### AMQP Wire-Level Format

#### Channel Multiplexing

#### Visibility Guarantee

#### Channel Closure

#### Content Synchronisation

#### Content Ordering Gurantees

#### Error Handling

#### Limitations

#### Security

## 小结