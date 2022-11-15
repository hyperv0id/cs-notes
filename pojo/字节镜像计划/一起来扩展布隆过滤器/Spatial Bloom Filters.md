空间布隆过滤器，可以在不经过地图服务提供商的情况下，告知用户其是否在某位置附近。

如果你不知道什么是布隆过滤器，请看看这篇文章：[Bloom Filter](../../../algo/bloom-filter/Bloom%20Filter.md)

## 构建

给定集合：$S=\left\{\Delta_{1}, \Delta_{2}, \ldots, \Delta_{s}\right\}$，其中每一项都是一个感兴趣的目标地点
在添加元素时，将 $\Delta_{1}$ 中的所有元素设为1，将 $\Delta_{2}$ 中的所有元素设为2，直到集合S被添加完
![](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/images/2022/11/15/20221115154352-93acd9.png)
在判断时，找到对应最小的值
