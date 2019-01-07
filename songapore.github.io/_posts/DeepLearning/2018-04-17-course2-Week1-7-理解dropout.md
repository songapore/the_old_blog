---
layout: post
title:  "理解 dropout（Understanding Dropout）-吴恩达 深度学习 course2 1.7笔记"
date:   2018-04-17 14:46:00
categories: DeepLearning
tags: DeepLearning 吴恩达
mathjax: true
---

* content
{:toc}

Dropout可以随机删除网络中的神经单元，他为什么可以通过正则化发挥如此大的作用呢？
<!--more-->


## 直观上理解
Dropout可以随机删除网络中的神经单元，他为什么可以通过正则化发挥如此大的作用呢？

![why does drop-out work](http://p5ocy6pck.bkt.clouddn.com/why-drop-out-work.png)

- 不依赖于任何一个特征，因为该单元的输入可能随时被清除
- 通过传播所有权重，dropout将产生收缩权重的平方范数的效果，和之前讲的L2正则化类似；

- 实施dropout的结果实它会压缩权重，并完成一些预防过拟合的外层正则化；

- L2对不同权重的衰减是不同的，它取决于激活函数倍增的大小。

总结一下，**dropout的功能类似于正则化**，与正则化不同的是，被应用的方式不同，dropout也会有所不同，甚至更适用于不同的输入范围。

## 实施dropout的细节
- 如果你担心某些层比其它层更容易发生过拟合，可以把某些层的keep-prob值设置得比其它层更低， 缺点是为了使用交叉验证，你要搜索更多的超级参数，
- 另一种方案是在一些层上应用dropout，而有些层不用dropout，应用dropout的层只含有一个超级参数，就是keep-prob。

## dropout常在计算机视觉应用中
- 计算视觉中的输入量非常大，输入太多像素，以至于没有足够的数据，所以dropout在计算机视觉中应用得比较频繁，有些计算机视觉研究人员非常喜欢用它，几乎成了默认的选择。
- 但要牢记一点，dropout是一种正则化方法，它有助于预防过拟合，除非算法过拟合，不然我是不会使用dropout的
- 它在其它领域应用得比较少，主要存在于计算机视觉领域，因为我们通常没有足够的数据，所以一直存在过拟合

## dropout 大缺点
dropout 的一大缺点是成本函数无法被明确定义。因为每次迭代都会随机消除一些神经元结点的影响，因此无法确保成本函数单调递减。因此，使用 dropout 时，先将keep_prob全部设置为 1.0 后运行代码，确保 `J(w,b)`函数单调递减，再打开 dropout
