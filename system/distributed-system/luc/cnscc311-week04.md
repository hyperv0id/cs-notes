# Distributed Systems What and Why

### 哪些是分布式系统

1. Windows记事本 
2. 手机微信。
3. 计算器
4. 网络搜索引擎——百度

不是:1、3
是：2、4

两种类型的主要区别?

- ds使用**不止一台电脑**——多少台?很难知道。
- 非ds -在单台计算机上运行

### 分布式系统特点

- 多台计算机
- 许多电脑不在你的口袋里，也不在你的桌面上——你甚至不知道它们在哪里!
- 不知何故，它们在后台**协同工作**，根据你的请求做一些事情!对你来说，它们只是一个单一的软件(比如你手机上的应用程序)。
- 计算机不共享一个全局时钟。消息传递而非共享内存

### 为什么要分布式系统

- 单个计算机的功率有限，分布式系统允许我们扩大**规模**——在更大的范围内做事情。
- 更**可靠**——数据和软件在多台计算机和站点上存储和复制，因此即使一台或两台计算机宕机，整个系统仍能正常运行。
- 正常运行**时间要求**——用户希望您的系统保持运行，单台计算机很难实现。
- 更好的**性能/延迟**-通过在多个站点上运行系统，用户可以使用更近的服务，这意味着更快。

## 分布式系统特点

### 资源共享

- 共享的资源：

  - 硬件：服务器、磁盘

  - 软件：文件、操作系统、数据

- 硬件共享的目的：

  - 便利性

  - 节约资源

- 数据共享：

  - 一致性：

  - 信息交换：更新数据库

  - 协作：共享文档

### 开放

开放分布式系统根据已发布的标准提供服务，这些标准通常使用**IDL(接口定义语言)**来指定服务的语法和语义。

如。

- 互联网是开放的系统——互联网协议是公开发布的。

- SOAP服务是开放系统——它的协议是用WSDL (Web服务描述语言)发布和指定的。



#### 开放的好处

- 互操作性——用不同语言编写并部署在不同计算机上的软件可以一起工作。
- 可移植性——软件可以很容易地移植到不同的操作系统上。
- 可扩展性——可以很容易地添加新服务，也可以很容易地更新旧服务。





### 并发性

- 多程序：几个程序同时运行。
- 多处理：在一台计算机中使用多个CPU。
- 并行执行：分布式系统中程序的并行执行。
- 资源共享：许多用户使用相同的资源和应用程序。

### 可伸缩性

#### 可扩展性类型

- 规模可扩展性-增加用户数量和资源
- 地理可扩展性-在更广泛的区域增加服务节点在不同的国家。


#### 良好的可扩展性

- 软件不应该为了支持增长而改变。当系统中的资源增加时，性能应该成比例地增加。

### 容错

- 正常工作：不会被故障影响到
- 优雅降级：性能降级，但降低得有限

### 透明度

- 定义：为对用户和应用程序开发人员隐瞒分布式系统中组件的分离，以便将系统视为一个整体。
- 访问透明性——使用相同的操作访问本地和远程资源。
- 位置透明性——资源在不知道其物理或网络位置的情况下被访问。
- 故障透明性——硬件和软件故障被隐藏，允许用户和应用程序开发人员完成他们的任务，尽管失败。
- 并发透明性——多个进程运行并发地使用共享资源而不相互干扰。

## CAP理论

- C: 一致性：所有节点的数据都是新的
- A：可用性：始终可操作
- P：分区容忍：节点故障或者延迟不影响整个系统

<img src="https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2023/11/26/image-20231126104315786-f47379.png" alt="image-20231126104315786" style="zoom:50%;" />

CAP中三个只能选择两个，不能完全满足



股票交易：CP，必须满足数据一致性



## 分布式系统的历史
