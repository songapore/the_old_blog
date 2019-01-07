---
layout: post
title:  "梯度检验应用的注意事项（Gradient Checking Implementation Notes）-吴恩达 深度学习 course2 1.14笔记"
date:   2018-04-19 20:40:10
categories: DeepLearning
tags: DeepLearning 吴恩达
mathjax: true
---

* content
{:toc}

在神经网络实施梯度检验的实用技巧和注意事项。
<!--more-->

## 在神经网络实施梯度检验的实用技巧和注意事项。

![Gradient Checking Implementation Notes](http://p5ocy6pck.bkt.clouddn.com/Gradient%20Checking%20Implementation%20Notes.png)

- 首先，不要在训练中使用梯度检验，它只用于调试。

我的意思是，计算所有i值的dθapprox[i]
是一个非常漫长的计算过程，为了实施梯度下降，你必须使用和 backprop来计算，并使用backprop来计算导数，只要调试的时候，你才会计算它，来确认数值是否接近。完成后，你会关闭梯度检验，梯度检验的每一个迭代过程都不执行它，因为它太慢了。

- 第二点，如果算法的梯度检验失败，要检查所有项，检查每一项，并试着找出bug。

也就是说，如果与dθ[i]的值相差很大，我们要做的就是查找不同的i值，看看是哪个导致dθapprox[i]与dθ[i]的值相差这么多。

- 第三点，在实施梯度检验时，当成本函数包含正则项时，也需要带上正则项进行检验；
- 第四点，梯度检验不能与dropout同时使用。

因为每次迭代过程中，dropout会随机消除隐藏层单元的不同子集，难以计算dropout在梯度下降上的代价函数。因此dropout可作为优化代价函数的一种方法，但是代价函数J被定义为对所有指数极大的节点子集求和。而在任何迭代过程中，这些节点都有可能被消除，所以很难计算代价函数。你只是对成本函数做抽样，用dropout，每次随机消除不同的子集，所以很难用梯度检验来双重检验dropout的计算，所以我一般不同时使用梯度检验和dropout。如果你想这样做，可以把dropout中的keepprob设置为1.0，然后打开dropout，并寄希望于dropout的实施是正确的，你还可以做点别的，比如修改节点丢失模式确定梯度检验是正确的。实际上，我一般不这么做，我建议关闭dropout，用梯度检验进行双重检查，在没有dropout的情况下，你的算法至少是正确的，然后打开dropout。
