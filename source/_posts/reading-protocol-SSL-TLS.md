---
title: reading-protocol-SSL-TLS
date: 2022-03-29 21:28:34
tags: HTTPS
categories: 
  - 学习
  - 协议
---
{% asset_img handshake.gif %}

本篇主要的内容是: 理解`SSL`和`TLS`是什么？作用是什么？它们之间的关系。

最近在阅读阿里的Sentinel的代码，在了解客户端和Dashboard的hearbeat与command时，看到了一段创建Socket的代码，这段代码大意是按照通信协议(HTTP/HTTPS)创建Socket，后续心跳检测和交互通过socket来通信。在创建HTTPS的Socket的代码片段中：`SSLContext.getInstance("TLS")`引起了我的好奇，于时想了解一下`TLS`到底时什么，在了解`TLS`的过程中，再次加深了一下HTTPS的对于消息加密的理解。

<!-- more -->
## 概述

`SSL`和`TLS`都属于加密协议，HTTPS为了保证通信的安全对消息的密钥进行了加密。它作于与客户端与服务端进行HTTPS通信的四次握手阶段。`TLS`可以看作是`SSL`的升级版本。

## 主要内容

在阅读了阮一峰老师的两篇文章[《SSL/TLS协议运行机制的概述》](https://www.ruanyifeng.com/blog/2014/02/ssl_tls.html)、[《图解SSL/TLS协议》](https://www.ruanyifeng.com/blog/2014/09/illustration-ssl.html)之后，生产了以下的笔记内容。

### SSL Handshake 握手阶段

{% asset_img ssl-handshake.png %}

1. SSL(Secure Sockets Layer, 安全套接字协议)与TLS(TransportLayerSecurity, 安全传输层)所解决的问题：防窃听(加密传播)、防篡改(校验机制)、防冒充(身份证书)。
2. SSL与TLS的历史：SSL是由网景(NetScape)公司1994年设计的，历经3个版本1.0(1994)、2.0(1995)、3.0(1996)。1999年互联网标准化组织(ISOC)接替了网景发布了SSL的升级版TLS 1.0。后续发布了1.1(2006)、1.2(2008)、1.2修正版(2011)。TLS的三个版本也被标注为SSL3.1、SSL3.2、SSL3.3。
3. SSL和TLS作用与建立连接阶段，客户端首先发起连接请求，通过4次握手生成3个随机数，用3个随机数生成用于加密报文的`对话密钥`(对称加密报文)。服务器通过公钥仅对`对话密钥`进行加密，而不对报文进行加密，来减少运算耗时。
4. 客户端首先发起握手请求，并生成第一个随机数(明文可见),服务端回应并生成第二个随机数(明文可见)，客户端收到回执后再次生成第三个随机数(用服务端公钥加密不可见)发给服务端，最后客户端和服务端通过三个随机数生成最终的`会话密钥`。
5. 为了保证公钥不被篡改，公钥放到了服务端的证书里。只要证书是可信的，公钥就是可信的。
6. 四次握手的过程中客户端与服务端也商定了加密方式，比如RSA公钥加密。

### Session恢复

握手阶段用来建立SSL连接。如果出于某种原因，对话中断，就需要重新握手。这时有两种方法可以恢复原来的session：一种叫做session ID，另一种叫做session ticket。

#### Session ID

{% asset_img session-id.png %}

session ID是指客户端给出session ID，服务器确认该编号存在，双方就不再进行握手阶段剩余的步骤，而直接用已有的对话密钥进行加密通信，它的缺点在于session ID往往只保留在一台服务器上。所以，如果客户端的请求发到另一台服务器，就无法恢复对话。

#### Session Ticket

{% asset_img session-ticket.png %}

session ticket是指客户端不再发送session ID，而是发送一个服务器在上一次对话中发送过来的session ticket。这个session ticket是加密的，只有服务器才能解密，其中包括本次对话的主要信息，比如对话密钥和加密方法。当服务器收到session ticket以后，解密后就不必重新生成对话密钥了。