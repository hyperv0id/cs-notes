## 5-Cache

问题：主存和CPU速度差距过大

解决办法：将主存的热点代码/数据保存到cache, cache采用SRAM实现，速度更快。

### 局部性原理

**空间局部性**：未来要用到的信息，很可能在最近信息的附近

**时间局部性**：最近的未来要使用的信息很可能就是当前信息



### 性能分析

$t_c$：访问一次`cache`的时间

$t_m$：访问一次主存的时间

==命中率==$H$：CPU想访问的信息，在`cache`的占比

==缺失率==$M$：未命中率。$M=1-H$

==平均访问时间==$t$：$$t = Ht_c + (1-H)(t_c+t_m)$$

![image-20230608102734086](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2023/06/08/image-20230608102734086-6c0a60.png)

### Cacha-主存映射方式

![image-20230608102948419](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2023/06/08/image-20230608102948419-26cb6c.png)

- 全相联
  - 在`cache`的任意位置，需要冗余信息(有效位+标记)。找cache时需要遍历所有cache
- 直接映射
  - `cache`号 = 主存号 % `Cache`总数
- 组相联
  - 将`Cache`每`M`个一组，共`N`组。主存放到组里的空余块中。

### Cache替换算法

#### RAND

随机策略：满了随便找一个替换

![image-20230608103814259](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2023/06/08/image-20230608103814259-5cdbe2.png)

#### FIFO

先进先出：替换最先被调用的块，有==抖动==

抖动：频繁的换入换出。

![image-20230608103745663](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2023/06/08/image-20230608103745663-fde8ee.png)

#### LRU

最近最少使用：	每个`Cache`设置**计数器**，记录多久没有防卫了，替换最远没用的。

![image-20230608104041258](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2023/06/08/image-20230608104041258-dc22c0.png)

#### LFU

最不经常使用：**计数器**记录被访问多少次，替换计数器少的。



### Cache-主存一致性

- 写命中（cache中有
  - 全写：每次写`Cache`都会写内存。会使用写缓冲区。
  - 写回：只写`Cache`，被替换时写回主存
- 写未命中（cache中没有
  - 写分配：调入`Cache`后改`Cache`
  - 非写分配：只写内存，不调入`Cache`

各级Cache之间采用：全写+非写分配

Cache-主存：写回+写分配



