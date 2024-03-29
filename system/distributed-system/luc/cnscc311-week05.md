# Data Replication, Distributed Transactions & DDBMS

## 分布式系统的挑战

网络传输过程中可能会发生错误，导致程序出错

![image-20231126110320542](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2023/11/26/image-20231126110320542-13e42e.png)



## 分布式数据复制

- 同步复制：更改后立即发送消息，通知其他节点
- 异步复制：仅在数据库中提交更改后，副本将被修改。

### 复制方案

- 完整的复制
	- 分布式通信网络中的几乎每个位置或用户都可以使用该数据库。
	- 优点:数据可用性高;更快地执行查询。
	- 缺点:并发控制困难;
- 不复制
	- 为没有复制数据库的每个片段只存储在一个位置。
	- 优点:可以优化并发性;数据易于恢复。
	- 缺点:数据可用性低;查询执行缓慢，因为多个客户端访问同一台服务器。
- 部分复制
	- 只复制数据库的一些片段。
	- 优点:副本的数量可以根据某些片段的重要性来控制，这是一个**平衡**的解决方案。


## 分布式事务

定义：并发访问/修改的同步机制

步骤：
- 谈判：要做什么
- 协商：任何一方都可以退出
- 达成协议：作出承诺

eg：![image.png](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2023/11/26/20231126123809-8ee924.png)

### 事务架构

![image.png](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2023/11/26/20231126123854-004a4e.png)

eg: ![image.png](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2023/11/26/20231126124333-249b77.png)

### 事务提交协议

#### 一阶段事务
- 协调器一直请求所有参与者提交，直到他们返回确认。
- 参与者**没有机会**发起中止。

#### 两阶段事务

- 阶段1:征集参与者**投票**。
	1. 协调器发送一个`canCommit`?请求所有参与者。接收时是否可以提交?
	2. 每个参与者回答`是`或`否`。
	3. 如果是，它将事务状态保存到**永久存储**之前发送yes回复。
	4. 如果没有，则立即终止。
- 阶段2:根据投票的输出完成提交或中止。
	1. 如果所有参与者回答yes(包括协调器)，协调器将`doCommit`发送给所有参与者;否则，协调器将`doAbort`发送给投票yes的参与者。
	2. 投赞成票的参与者等待协调器的`doCommit`或`doAbort`，并采取相应的行动。在`doCommit`的情况下，它们发送一个`havecommitted`确认。
- 参与者**可以**发起中止。



## 分布式数据库管理系统 DDBMS

- 对分布式、碎片化和复制数据的**透明**管理。
- 通过分布式事务提高**可靠**性和**可用**性。
- 支持更容易和经济的系统**扩展**。
- 可以在系统**故障**中存活，而不会丢失数据库中的数据。

### 架构

#### CS
1. 在网络中连接的许多客户机和一些服务器。
2. 客户端向其中一个服务器发送查询，最早可用的服务器解决问题并回复。
3. 作为集中式服务器系统易于实现。

#### 协作服务器
1. 设计用于在多个服务器上运行单个查询。
2. 服务器将单个查询分解为多个小查询，并组合子结果并将其发送回客户端。
3. 每个服务器都可以跨数据库执行当前事务。
4. 有点像 Map-Reduce

#### [中间件](../../../wiki/中间件.md)

1. 单个查询在多个服务器上执行。
2. 该系统需要一个能够管理来自其他服务器的查询和事务的服务器(作为[中间件](../../../wiki/中间件.md))。
3. 本地服务器处理本地查询和事务。



#### Single Store 数据库

1. SingleStore数据库可以**分布**在多个机器和位置。
2. 数据存储在叶节点上的分区中，每个分区都包含主版本和从版本。
3. 如果主服务器故障，从服务器接管一个软件管理聚合器和叶节点。
4. 聚合器节点负责接收SQL查询，将其分解到叶节点上，并将结果合并回客户端。
5. 聚合器和叶节点使用SQL进行通信。

### QA

> [!question]
> 如果要求您构建一个在线购物系统，哪种DDBMS体系结构最适合您?
> 
> > [!summary]-
> > CS架构
> 





