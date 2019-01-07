---
layout: post
title:  "学习率衰减(Learning rate decay)-吴恩达 深度学习 course2 2.9笔记"
date:   2018-05-06 14:30:10
categories: DeepLearning
tags: DeepLearning 吴恩达
mathjax: true
---
* content
{:toc}

<!--more-->

加快学习算法的一个办法就是随时间慢慢减少学习率，我们将之称为**学习率衰减**，我们来看看如何做到。

## 为什么要计算学习率衰减
假设你要使用mini-batch梯度下降法，mini-batch数量不大，大概64或者128个样本，在迭代过程中会有噪音（蓝色线），下降朝向这里的最小值，但是不会精确地收敛，所以你的算法最后在附近摆动，并不会真正收敛，因为你用的α是固定值，不同的mini-batch中有噪音。

![image](http://p5ocy6pck.bkt.clouddn.com/why%20computer%20learning%20rate%20decay.png)

但要慢慢减少学习率α的话，在初期的时候，学习率α还较大，你的学习还是相对较快，但随着α变小，你的步伐也会变慢变小，所以最后你的曲线（绿色线）会在最小值附近的一小块区域里摆动，而不是在训练过程中，大幅度在最小值附近摆动。

所以慢慢减少的本质在于，在学习初期，你能承受较大的步伐，但当开始收敛的时候，小一些的学习率能让你步伐小一些。

## 最常用的学习率衰减方法：

$$
\alpha = \frac{1}{1 + decay\_rate * epoch\_num} * \alpha_0
$$
其中，decay_rate为衰减率（超参数），epoch_num为将所有的训练样本完整过一遍的次数。注意这个衰减率是另一个你需要调整的超参数

要理解，作为代数函数，根据上述公式，你的学习率呈递减趋势。如果你想用学习率衰减，要做的是要去尝试不同的值，找到合适的值，除了这个学习率衰减的公式，人们还会用其它的公式。
## 其它的学习率衰减方法

![other learning rate decay methods](http://p5ocy6pck.bkt.clouddn.com/Other%20learning%20rate%20decay%20methods.png)

- 指数衰减：

$$
\alpha = 0.95^{epoch\_num} * \alpha_0
$$
- 其他：

$$
\alpha = \frac{k}{\sqrt{epoch\_num}} * \alpha_0
$$
- 离散下降
