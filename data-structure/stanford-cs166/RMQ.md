---
tags: ["  #data-structure "]
aliases: [Range Minimize Query, 范围最小查询]
---
范围最小查询（Range Minimize Query）

## 问题描述
给定任意数组 $A$ ，对于任意 $0\leq i<j<A.size()$ 请给出区间 $i-j$ 的最小值
![](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/images/2022/12/02/20221202204133-1410bb.png)

## Solutions

### 硬算

直接从 i 到 j 遍历，找到最小的

时间复杂度：$O(N)$
空间复杂度：$O(1)$

### 打小抄

提前记录每个查询，到查询时再找

![](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/images/2022/12/02/20221202204518-c528ea.png)时间复杂度：$O(1)$，构建时间$O(N^3)$
空间复杂度：$O(1)$

这里可以使用动态规划加速：

![image-20221202205056974](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2022/12/02/image-20221202205056974-f659c1.png)

时间复杂度：$O(1)$，构建时间$O(N^2)$
空间复杂度：$O(1)$

### 打缩印-Block Decomposition

将元素数组分成 $b$ 个桶，每个桶里面求出最小值。

查询时：如果横跨整个桶，就按记录的桶中最小值来，反之还是要计算的。

![image-20221202205225957](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2022/12/02/image-20221202205225957-4fafc3.png)

- 时间复杂度：$O(b + n/b)$，$O(b)$时间查询整个桶，$O(n/b)$查询分桶的
- 构建时间：$O(b) * O(n/b) = O(n)$
- 空间复杂度：$O(b)$
- 最佳的$b$: $b = \sqrt{n}$，这时时间复杂度：$O(n^{0.5})$

### Sparse Tables

对于每个索引i，计算从i开始的范围的RMQ，大小为1、2、4、8、16、…、$2^k$,直到越界

观察下面整个情况，2-7之间的最小值可以通过两个更小的子问题求出来。

![image-20221202210515432](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2022/12/02/image-20221202210515432-19af83.png)

那么只需要可以一个更小的备忘录来查询

![image-20221202211643246](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2022/12/02/image-20221202211643246-35e048.png)

这个备忘录保存了从i开始长度为$2^k$的子数组的最小值，而且这个备忘录也可以用动态规划求解出来。

时间复杂度：$O(1)$，构建$O(n\log{n})$

空间复杂度：$O(n\log{n})$

### 组合查询

采用分桶的思想，将整个数组的RMQ分解成几个子RMQ，父子RMQ问题会采用不同的策略（比如线性，sparse），甚至父子RMQ问题都可以采用组合查询。

![image-20221202212352847](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2022/12/02/image-20221202212352847-01fa50.png)

我们采用记号 $<p_1(n), q_1(n)>, \quad <p_2(n), q_2(n)>$分别表示父子查询的构建耗时和单次查询耗时。（p=precompute，q=query）

那么组合查询的查询复杂度为 $O(q_1(n/b) + q_2(b))$，总构建时间 $O(n + p_1(n / b) + (n / b)p_2(b))$，通过组合可构造出更小复杂度的算法

#### 一些组合例子
比如组合sparse与线性

![image-20221202212550223](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2022/12/02/image-20221202212550223-8f4013.png)

构建：$O(n)$，查询$O(\log{n})$

全用sparse

![image-20221202212659046](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2022/12/02/image-20221202212659046-5bacf7.png)

构建：$O(n\log\log{n})$，查询$O(1)$


### 笛卡尔树
**笛卡尔树**可以在 $<O(n), O(1)>$ 的复杂度上解决RMQ问题。

#### 问题转换
> 组合查询的查询复杂度为 $O(q_1(n/b)+ q_2(b))$，总构建时间 $O(n + p_1(n / b) + (n / b)p_2(b))$，记作 $<O(q_1(n/b)+ q_2(b)), O(n + p_1(n / b) + (n / b)p_2(b))>$

那么如果要构建复杂度为 $<O(n), O(1)>$ 的算法，$p_2$ 和 $q_2$ 的复杂度为多少？

> $p_2(n) = O(n),\quad q_2(n) = O(1)$

于是问题转换为：

能否在 $O(n)$ 时间构建数据结构，满足在很多个子数组中查询最小值**下标**，且查询时间复杂度为 $O(1)$。（这里可以把单个数字看作长度为1的数组

为什么这个问题会比之前的问题更简单？举个例子：

```
两个数组
10	30	20	40
166	361	261	464

无论你如何选择i，j，返回下标都一样！！！
```

这意味者RMQ问题在这两个数组上是**等价**的，返回的坐标相同，这里我们记作**RMQ结构**相同。

那么子RMQ问题就可以转换成有限个更小的RMQ问题，比如下面这个图。
![](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/images/2022/12/03/20221203193009-a56f30.png)

#### 如何判断RMQ结构是否相同

假设数组 B1 B2 长度都为 b，只要他们满足
$$
\forall 0\leq i \leq j < b \quad
RMQ_{B_1}(i, j) = RMQ_{B_2}(i, j)
$$
就称其等价，记作 $B1 \sim B2$

只要 $B1 \sim B2$，那么最小值下标就相同，而且可以递归的应用到子数组上！

笛卡尔树结构如下:

- 树的根是数组的最小元素。
- 它的左右子树是通过递归构建形成的
- 笛卡尔树的子数组的左、右最小值。

<img src="https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2022/12/03/image-20221203194719734-084a2e.png" alt="image-20221203194719734" style="zoom:67%;" />

那么是不是只要笛卡尔树形状相同，就可以说明俩数组等价啦



#### 构建

构建复杂度如下：

![image-20221203201630345](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2022/12/03/image-20221203201630345-de0825.png)

计算最小值$O(n)$，数组长为 $b$ 的笛卡尔树一共 $4^b$ 种，当 $b = k \log_{4}{b}$ 且 $k<1$ 时复杂度不超过 $O(n)$，总体复杂度 $O(1)$

#### 查询



![image-20221203195620656](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2022/12/03/image-20221203195620656-370f39.png)

## 例题

- [654. 最大二叉树 | 力扣](https://leetcode.cn/problems/maximum-binary-tree/solutions/)
	- [C++ 8 lines O(n log n) map, plus stack with binary search - LeetCode Discuss](https://leetcode.com/problems/maximum-binary-tree/discuss/106147/c-8-lines-on-log-n-map-plus-stack-with-binary-search)
- 





