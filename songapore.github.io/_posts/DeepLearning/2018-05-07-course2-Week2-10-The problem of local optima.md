---
layout: post
title:  "局部最优的问题(The problem of local optima)-吴恩达 深度学习 course2 2.10笔记"
date:   2018-05-07 14:58:30
categories: DeepLearning
tags: DeepLearning 吴恩达
mathjax: true
---
* content
{:toc}

<!--more-->

在深度学习研究早期，人们总是担心优化算法会困在极差的局部最优，不过随着深度学习理论不断发展，我们对局部最优的理解也发生了改变。我向你展示一下现在我们怎么看待局部最优以及深度学习中的优化问题。

![The problem of local optima](http://p5ocy6pck.bkt.clouddn.com/the%20problem%20of%20local%20optima.png)

这是曾经人们在想到局部最优时脑海里会出现的图，也许你想优化一些参数，我们把它们称之为W1和W2，平面的高度就是损失函数。在图中似乎各处都分布着局部最优。梯度下降法或者某个算法可能困在一个局部最优中，而不会抵达全局最优。如果你要作图计算一个数字，比如说这两个维度，就容易出现有多个不同局部最优的图，而这些低维的图曾经影响了我们的理解，但是这些理解并不正确。

事实上，如果你要创建一个神经网络，通常梯度为零的点并不是这个图中的局部最优点，实际上成本函数的零梯度点，通常是鞍点。

![saddle](http://p5ocy6pck.bkt.clouddn.com/saddle.png)

所以我们从深度学习历史中学到的一课就是，我们对低维度空间的大部分直觉，比如你可以画出上面的图，并不能应用到高维度空间中。适用于其它算法，因为如果你有2万个参数，那么函数有2万个维度向量，你更可能遇到鞍点，而不是局部最优点。

鞍点（saddle）是函数上的导数为零，但不是轴上局部极值的点。当我们建立一个神经网络时，通常梯度为零的点是上图所示的鞍点，而非局部最小值。减少损失的难度也来自误差曲面中的鞍点，而不是局部最低点。因为在一个具有高维度空间的成本函数中，如果梯度为 0，那么在每个方向，成本函数或是凸函数，或是凹函数。而所有维度均需要是凹函数的概率极小，因此在低维度的局部最优点的情况并不适用于高维度。

结论：

- 在训练较大的神经网络、存在大量参数，并且成本函数被定义在较高的维度空间时，困在极差的局部最优中是不大可能的；
- 鞍点附近的平稳段会使得学习非常缓慢，而这也是动量梯度下降法、RMSProp 以及 Adam 优化算法能够加速学习的原因，它们能帮助尽早走出平稳段。