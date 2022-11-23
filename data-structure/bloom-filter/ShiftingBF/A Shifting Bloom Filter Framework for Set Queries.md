---
tags: ["#data-structure #data-structure/bloom-filter"]
---
# A Shifting Bloom Filter Framework for Set Queries

[TOC]



## 摘要



## 1-简介



### 1.1-动机



### 1.2-提出的方法

本文提出了一个*移位布隆过滤器*（***Shifting Bloom Filter***）来表示并进行集合查询，简称**ShBF**

我们创建 $k$ 个独立的均匀分布的哈希函数 $h_1(.), \dots, h_k(.)$ ，在构建时初始化长度为 $m$ 的位图 $B$ ，对于每个元素 $e$，我们保留其存在信息和辅助信息

- 存在信息：$e$ 是否在 set 中，本文中为 $h_1(e) \% m, \dots, h_k(e) \% m$ 
- 辅助信息：一些额外信息，如出现次数、出现在那一个set中。本文表示为 $o(e)$。

在**ShBF**中，与BF不同，我们会将下列比特位设为1

1. $h_1(e)\%m, \dots h_k(e)\%m$ 
2. $[h_1(e) + o(e)]\%m, \dots, [h_k(e) + o(e)]\%m$  



![image-20221107142733641](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2022/11/07/image-20221107142733641-245790.png)



#### 1.2.1-成员查询





#### 1.2.2-关联查询





#### 1.2.3-多重性查询





### 1.3-优势





## 2-相关工作



### 2.1-成员查询





### 2.2-关联查询





### 2.3-多重性查询





## 3-成员查询





## 4-关联查询





## 5-多重性查询





## 6-性能测试