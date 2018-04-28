---
title: java-nio-notes
date: 2018-04-20 18:36:53
tags: NIO
categories: 
  - 笔记
  - JAVA
---
{% asset_img nio.jpg %}   

本篇主要的内容是: Java中NIO知识点的总结整理。参考链接：http://ifeve.com/overview/

<!-- more -->

## 概述
Java NIO 核心组成部分
* Channels
* Buffers
* Selectors

**Channel 和 Buffer**

主要的Channel的实现
* FileChannel - 从文件中读写数据
* DatagramChannel - 能通过UDP读写网络中的数据
* SocketChannel - 能通过TCP读写网络中的数据
* ServerSocketChannel - 可以监听新进来的TCP连接，像WEB服务器那样。对每一个新进来的连接都会创建一个SocketChannel。

以上这些通道涵盖了UDP和TCP网络IO，以及文件IO

关键的Buffer实现
* ByteBuffer
* CharBuffer
* DoubleBuffer
* FloatBuffer
* IntBuffer
* LongBuffer
* ShortBuffer

以上这些Buffer涵盖了IO发送的基本数据类型：byte、short、int、long、float、double和char。

**Selector**

Selector允许单线程处理多个Channel。要使用Selector,需要向Selector注册Channel，然后调用select()方法。

## 主要内容

### Channel

通道。类似流
* 既可以从通道中读取数据，又可以写数据到通道。
* 可以异步的读写。
* 通道中的数据总是要先读到一个Buffer，或者总是从一个Buffer中写入

### Buffer

缓存。Buffer的读写操作一般遵循以下四个步骤

1. 写入数据到Buffer
2. 调用flip()方法切换写模式为读模式
3. 从Buffer中读取数据
4. 调用clear()方法或compact()方法清理缓存

当向Buffer写入数据时，Buffer会记录下来写了多少数据。当到读取数据时，需要通过flip()方法切换Buffer的写模式为读模式，然后既可以读取Buffer中的所有数据。读完数据之后，需要清理缓存，让它可以被再次写入。清理缓存有两种模式：
* 调用clear()方法
* 调用compact()方法

clear()方法会清空全部的缓存，compact()方法只会清楚读取过的缓存。如果使用compact()清理缓存后，再次向缓存写入数据，则缓存中未读过的数据置顶，新写的数据将放到缓存区未读后面继续写入。

那么怎么知道我们缓存有多大？现在读到了多少?

这里会涉及到Buffer的三个属性
* capacity
* position
* limit

**capacity ：** 该属性标识当前缓存大固定大小值。

**position ：** 在写模式下，该属性的初始值是0，它会根据数据的写入逐渐增长，最大为capacity-1。在读模式下，该属性会重新指向0，它会根据数据的读取逐渐增大，当从Buffer的position处读取一个数据只，position会向前移动到下一个可读位置。

**limit ：** 在写模式下，limit表示最多可以往Buffer中写多少数据，limit的值等于capacity的值。在读模式下，limit表示可以读到的数据的大小。

Buffer常用方法
* Buffer大小得分配 - **Buffer.allocate(int num)**
* Buffer数据的写入 - **channel.read(Buffer buf)** or **Buffer.put**
* Buffer数据的读取 - **Buffer.get()** or **channel.write(Buffer buf)**
* Buffer读写模式的切换 - **Buffer.flip()**
* Buffer的重读 - **Buffer.rewind()**
* Buffer的清理 - **Buffer.clean** and **Buffer.compact()**
* Buffer属性posititon的标识和复位 - **Buffer.mark()** and **Buffer.reset**
* Buffer的比较 - **equals()** and **compareTo()**

> **NOTE :** equals()方法之比较Buffer中剩余数据的类型、个数、和每个剩余数据中每个元素的类型和大小。compareTo()比较Buffer大小的原则：一、比较第一个不相等元素的大小，二、比较两个Buffer剩余容量。  

> **NOTE :** 上面所说的剩余是position到limit之间的数据，Buffer的读写模式切换，会导致这两个值的变化，同时也会影响equals和compareTo方法的结果。

## 小结