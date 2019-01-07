---
layout: post
title:  "Adam 优化算法(Adam optimization algorithm)-吴恩达 深度学习 course2 2.8笔记"
date:   2018-05-02 17:20:10
categories: DeepLearning
tags: DeepLearning 吴恩达
mathjax: true
---

* content
{:toc}

Adam 优化算法（Adaptive Moment Estimation，自适应矩估计）基本上就是将 Momentum 和 RMSProp 算法结合在一起，通常有超越二者单独时的效果。
<!--more-->



在深度学习的历史上，包括许多知名研究者在内，提出了优化算法，并很好地解决了一些问题，但随后这些优化算法被指出并不能一般化，并不适用于多种神经网络。

RMSprop以及Adam优化算法，就是少有的经受住人们考验的两种算法，已被证明适用于不同的深度学习结构，这个算法我会毫不犹豫地推荐给你，因为很多人都试过，并且用它很好地解决了许多问题。
## 实现
**Adam 优化算法**（Adaptive Moment Estimation，自适应矩估计）基本上就是将 Momentum 和 RMSProp 算法结合在一起，通常有超越二者单独时的效果。那么来看看如何使用Adam算法

![Adam optimization algorithm](http://p5ocy6pck.bkt.clouddn.com/Adam%20optimization%20algorithm.png)

使用Adam算法，首先你要初始化：

$$
v_{dW} = 0, s_{dW} = 0, v_{db} = 0, s_{db} = 0
$$

用每一个 mini-batch 计算 dW、db，第 t 次迭代时

$$
v_{dW} = \beta_1 v_{dW} + (1 - \beta_1) dW
$$

$$
v_{db} = \beta_1 v_{db} + (1 - \beta_1) db
$$

$$
s_{dW} = \beta_2 s_{dW} + (1 - \beta_2) {(dW)}^2
$$

$$
s_{db} = \beta_2 s_{db} + (1 - \beta_2) {(db)}^2
$$

一般使用 Adam 算法时需要计算偏差修正：

$$
v^{corrected}_{dW} = \frac{v_{dW}}{1-{\beta_1}^t}
$$

$$
v^{corrected}_{db} = \frac{v_{db}}{1-{\beta_1}^t}
$$

$$
s^{corrected}_{dW} = \frac{s_{dW}}{1-{\beta_2}^t}
$$

$$
s^{corrected}_{db} = \frac{s_{db}}{1-{\beta_2}^t}
$$

所以，更新 W、b 时有：


$$
W := W - \alpha \frac{v^{corrected}_{dW}}{\sqrt{s^{corrected}_{dW}} + \epsilon}
$$

$$
b := b - \alpha \frac{v^{corrected}_{db}}{\sqrt{s^{corrected}_{db}} + \epsilon}
$$

（可以看到 Andrew 在这里 ϵ 没有写到平方根里去，和他在 RMSProp 中写的不太一样。考虑到 ϵ 所起的作用，我感觉影响不大）

## 超参数选择

![超参数选择](http://p5ocy6pck.bkt.clouddn.com/hyperparameters%20choice.png)

Adam 优化算法有很多的超参数，其中
- 学习率 α：需要尝试一系列的值，来寻找比较合适的；
- β1：常用的缺省值为 0.9；
- β2：Adam 算法的作者建议为 0.999；
- ϵ：不重要，不会影响算法表现，Adam 算法的作者建议为 10−8；

β1、β2、ϵ 通常不需要调试。
