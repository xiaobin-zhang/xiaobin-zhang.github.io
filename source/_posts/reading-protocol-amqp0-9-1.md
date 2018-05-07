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