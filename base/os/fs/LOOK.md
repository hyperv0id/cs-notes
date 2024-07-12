---
aliases:
  - LOOK算法
  - LOOK调度
  - C-LOOK
  - 循环电梯
---


LOOK算法（也称为C-LOOK或循环电梯算法）是SCAN算法的一种改进，它试图解决SCAN算法在某些情况下可能产生的性能问题。LOOK算法的核心思想是在改变磁头移动方向之前，先检查当前磁头位置的对面是否有请求，如果有，就先服务对面的请求，然后再改变方向。

## 工作原理

1. **初始化方向**：与SCAN算法类似，LOOK算法开始时，磁头可以向两个方向之一移动。
2. **请求队列**：磁盘访问请求按照它们在磁盘上的位置被组织成一个队列。
3. **移动磁头**：磁头沿着当前方向移动，访问请求队列中的磁盘块。
4. **检查对面请求**：在磁头到达磁盘的一端之前，先检查对面是否有请求。如果有，磁头会先移动到对面服务这些请求。
5. **到达端点**：如果对面没有请求，磁头到达磁盘的一端后，改变移动方向。
6. **继续访问**：改变方向后，磁头继续移动并访问另一侧的请求队列中的磁盘块。
7. **循环**：这个过程会一直持续，直到所有请求都被服务。

## 优缺点

### 优点

- **减少回溯**：通过检查对面是否有请求，LOOK算法可以减少磁头的回溯次数，提高效率。
- **提高响应速度**：对于靠近磁头当前位置的请求，LOOK算法可以更快地响应。

### 缺点

- **实现复杂性**：LOOK算法需要额外的逻辑来检查对面是否有请求，这增加了算法的实现复杂性。
- **性能波动**：在磁盘请求分布不均匀的情况下，LOOK算法的性能可能会有波动。

## 应用场景

LOOK算法适用于磁盘请求分布不均匀的情况，尤其是在磁盘的一端有大量请求，而另一端请求较少时，LOOK算法可以提供更好的性能。

## 变种

- **[C-SCAN](C-SCAN.md)**：LOOK算法可以与[C-SCAN](C-SCAN.md)算法结合，形成C-LOOK算法，进一步提高性能。

LOOK算法是一种动态的磁盘调度策略，它通过智能地检查对面的请求来减少磁头的移动距离和回溯次数，从而提高磁盘的访问效率。然而，它可能需要更复杂的实现和更多的计算资源。在实际应用中，需要根据磁盘的访问模式和性能要求来选择最合适的磁盘调度算法。



## 例子



![在这里插入图片描述](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2024/07/06/20200408181748251-76aec5.png)