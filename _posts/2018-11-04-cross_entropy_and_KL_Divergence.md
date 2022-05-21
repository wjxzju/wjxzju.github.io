---
title: 交叉熵与KL散度概念
tags: 机器学习
mathjax: true
---

## 交叉熵

熵的本质就是信息量的期望，在本科通信原理课程中有讲述，随机变量的不确定性越大，熵也就越大，计算公式为

$$
H(p) = - \sum_{i} p(i) log p(i)
$$

交叉熵是通过非真实分布`q`来表示真实分布`p`的平均编码长度， 计算公式为

$$
H(p,q) = - \sum_i p(i)  log q(i) 
$$

根据吉布斯不等式有$ H(p,q) \ge H(p)$恒成立， 当`q`取真实分布`p`时，等式成立。

## KL散度

KL散度的计算公式

$$
D(p\|q) = H(p,q) - H(p) = \sum_i p(i) log \frac{p(i)}{q(i)}
$$

交叉熵经常在机器学习中作为损失函数使用，用来评估模型预测的分布与真实分布的相似性。

### 参考

- [知乎的回答](https://www.zhihu.com/question/41252833)
- [StackExchange的一个回答](https://stats.stackexchange.com/questions/265966/why-do-we-use-kullback-leibler-divergence-rather-than-cross-entropy-in-the-t-sne)
