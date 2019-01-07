---
layout: post
title:  "神经网络的权重初始化（Weight Initialization for Deep Networks）-吴恩达 深度学习 course2 1.11笔记"
date:   2018-04-19 20:25:10
categories: DeepLearning
tags: DeepLearning 吴恩达
mathjax: true
---

* content
{:toc}

针对梯度消失和梯度爆炸问题，我们想出了一个不完整的解决方案
<!--more-->


针对梯度消失和梯度爆炸问题，我们想出了一个不完整的解决方案，虽然不能彻底解决问题，却很有用，有助于我们为神经网络更谨慎地选择随机初始化参数。

![Weight Initialization](http://p5ocy6pck.bkt.clouddn.com/Weight%20Initialization.png)

当输入的数量 n 较大时，我们希望每个 wi 的值都小一些，这样它们的和得到的 z 也较小。

要得到较小的 wi，设置Var(wi)=1/n，这里称为 **Xavier initialization**。


```
WL = np.random.randn(WL.shape[0], WL.shape[1]) * np.sqrt(1/n)
```
n就是我喂给的神经单元数量

这样，激活函数的输入 x 近似设置成均值为 0，标准方差为 1，相应的，神经元输出 z 的方差就正则化到 1 了。虽然没有解决梯度消失和爆炸的问题，但其在一定程度上确实减缓了梯度消失和爆炸的速度。

将 ReLU 作为激活函数时，Var(wi)=2/n

 总结，当激活函数使用 ReLU 时，Var(wi)=2/n；当激活函数使用 tanh 时，Var(wi)=1/n。

 实际上，我认为所有这些公式只是给你一个起点，它们给出初始化权重矩阵的方差的默认值
