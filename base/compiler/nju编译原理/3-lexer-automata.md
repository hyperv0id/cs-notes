# 自动机理论 & 词法分析器生成器

问题：如何从[正则表达式](../正则表达式.md)转换成词法分析器？

## 自动机

### 两大要素

1. 状态集：$S$
2. 状态转移函数: $\sigma$
### 状态机分层
自动机根据表达/计算能力的强弱可以分为4个层次，本文讨论**有穷状态机**
![image.png](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2023/12/29/20231229172059-5beb3f.png)

### 从[正则表达式](../正则表达式.md)到词法分析器


目标：`RE => NFA => DFA => 词法分析器`

![image.png](https://pic-1257412153.cos.ap-nanjing.myqcloud.com/images/2023/12/29/20231229172158-3a9bf1.png)

- 从[正则表达式](../正则表达式.md)到[NFA](../NFA.md)这一步我们使用[Thompson构造法](../Thompson构造法.md)
- 从[NFA](../NFA.md)转换到[DFA](../DFA.md)：[子集构造法](../子集构造法.md)
- [DFA最小化](../DFA最小化.md)：合并等价状态
- [DFA](../DFA.md)回到[正则表达式](../正则表达式.md)：[Kllene构造法](../Kllene构造法.md)

## 词法分析器生成器

