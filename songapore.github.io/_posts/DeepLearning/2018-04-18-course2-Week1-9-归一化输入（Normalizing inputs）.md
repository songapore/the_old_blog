---
layout: post
title:  "归一化输入（Normalizing inputs）-吴恩达 深度学习 course2 1.9笔记"
date:   2018-04-18 10:30:10
categories: DeepLearning
tags: DeepLearning 吴恩达
mathjax: true
---

* content
{:toc}

训练神经网络，其中一个加速训练的方法就是归一化输入(Normalizing inputs)
<!--more-->


## 归一化输入的两个步骤
训练神经网络，其中一个**加速训练的方法就是归一化输入(Normalizing inputs)**。假设一个训练集有两个特征，输入特征为2维，归一化需要两个步骤：

1. 零均值化

2. 归一化方差；

我们希望无论是**训练集和测试集都是通过相同的均值和方差来完成归一化**

![Normalizing inputs](http://p5ocy6pck.bkt.clouddn.com/Normalizing%20inputs.png)

- 标准化公式:

$$
x = \frac{x - \mu}{\sigma}
$$

其中，

$$
\mu = \frac{1}{m}\sum^m_{i=1}{x^{(i)}}
$$

$$
\sigma = \sqrt{\frac{1}{m}\sum^m_{i=1}{x^{(i)^2}}}
$$

## 为什么要归一化特征输入

![image](http://p5ocy6pck.bkt.clouddn.com/why%20Normalizing%20inputs.png)

在不使用标准化的成本函数中，如果设置一个较小的学习率，可能需要很多次迭代才能到达全局最优解；而如果使用了标准化，那么无论从哪个位置开始迭代，都能以相对较少的迭代次数找到全局最优解。
