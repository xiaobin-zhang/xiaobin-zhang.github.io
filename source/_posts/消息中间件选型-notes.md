---
title: 消息中间件选型-notes
date: 2018-04-28 10:17:10
tags: MQ
categories: 
  - 笔记
  - MQ
---
{% asset_img nio.jpg %}   

本篇主要的内容是: 阅读了公众号-“程序猿DD”的文章[《消息中间件选型分析》][1]后，记录的笔记。

<!-- more -->

## 概述
在LZ工作当中，也涉及到消息中间件的使用，目前项目使用的是RattitMq。在很长一段时间内，对消息中间件的了解还是非常模糊的，在阅读了公众号-“程序猿DD”的文章[《消息中间件选型分析》这篇文章后，对消息中间件的概念上，有了基本的概念，接下来是LZ觉得需要记录的部分。

## 主要内容

### 消息中间件的种类

目前市面上的消息中间件的种类有以下几个
* ActiveMQ
* RabbitMQ
* Kafka
* RocketMQ
* ZeroMQ

### 选型要点

在消息中间件的选型上，主要选型要点有以下几个
* 功能维度
* 性能
* 可靠性+可用性
* 运维管理
* 社区力度及生态发展

#### 功能维度
* 优先级队列
* 延迟队列
* 死信队列
* 重试队列
* 消费模式
* 广播消费
* 消息回溯
* 消息堆积+持久化
* 消息追踪
* 消息过滤
* 多租户
* 多协议支持
* 跨语言支持
* 流量控制
* 消息顺序性
* 安全机制
* 事务性消息

[1]:https://mp.weixin.qq.com/s?__biz=MzAxODcyNjEzNQ==&mid=2247485037&idx=1&sn=7ab8eeb739c479006d1ee48440bcbbb2&chksm=9bd0abf5aca722e3cb9f04fd3ea9ece96bf1484bef4ebb3bf20950897cc3ddaca79f871c4181&mpshare=1&scene=1&srcid=04283ZIwKgrzz3Jb15mGoHLV#rd