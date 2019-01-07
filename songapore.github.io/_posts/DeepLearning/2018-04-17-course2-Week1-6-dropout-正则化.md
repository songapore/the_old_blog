---
layout: post
title:  "dropout 正则化（Dropout Regularization）-吴恩达 深度学习 course2 1.6笔记"
date:   2018-04-17 14:43:00
categories: DeepLearning
tags: DeepLearning 吴恩达
mathjax: true
---

* content
{:toc}

假设你在训练左图这样的神经网络，它存在过拟合，这就是dropout所要处理的，我们复制这个神经网络，dropout会遍历网络的每一层，并设置消除神经网络中节点的概率。
<!--more-->



## dropout工作原理
除了L2 正则化，还有一个非常实用的正则化方法——“**Dropout（随机失活）**”，我们来看看它的工作原理。
![image](http://p5ocy6pck.bkt.clouddn.com/dropout_regularization.png)

假设你在训练左图这样的神经网络，它存在过拟合，这就是dropout所要处理的，我们复制这个神经网络，**dropout会遍历网络的每一层，并设置消除神经网络中节点的概率**。假设网络中的每一层，每个节点都以抛硬币的方式设置概率，每个节点得以保留和消除的概率都是0.5，设置完节点概率，我们会消除一些节点，然后删除掉从该节点进出的连线，最后得到一个节点更少，规模更小的网络，然后用backprop方法进行训练。


右图是网络节点精简后的一个样本，对于其它样本，我们照旧以抛硬币的方式设置概率，保留一类节点集合，删除其它类型的节点集合。对于每个训练样本，我们都将采用一个精简后神经网络来训练它，这种方法似乎有点怪，单纯遍历节点，编码也是随机的，可它真的有效。不过可想而知，我们针对每个训练样本训练规模极小的网络，最后你可能会认识到为什么要正则化网络，因为我们在训练极小的网络

## 如何实施dropout呢
如何实施dropout呢？方法有几种，接下来我要讲的是最常用的方法，即**inverted dropout（反向随机失活）**，出于完整性考虑，我们用一个三层（`l=3`）网络来举例说明。

![如何实施dropout](http://p5ocy6pck.bkt.clouddn.com/dropuout.png)

- 首先要定义向量`d`

```
d3 = np.random.rand(a3.shape[0],a3.shape[1])
```
- 然后看它是否小于**keep-prob**

```
d3 = np.random.rand(a3.shape[0],a3.shape[1]) < keep_prob
```
keep-prob是一个具体数字，它表示**保留某个隐藏单元的概率**.

如果用python实现该算法的话，d3则是一个布尔型数组，值为true和false

- 获取激活函数

```
a3 =np.multiply( a3,d3)
```

- 最后,除以keep-prob参数

```
a3 /= keep_prob
```
为了不影响`z4`的期望值，我们需要用`a3 /= keep_prob`，它将会修正或弥补我们所需的那20%，`a3 `的期望值不会变。**反向随机失活（inverted dropout）方法通过除以keep-prob，确保的期望值不变。**

据我了解，目前实施dropout最常用的方法就是Inverted dropout，建议大家动手实践一下

在测试阶段，我们并未使用dropout，自然也就不用抛硬币来决定失活概率，以及要消除哪些隐藏单元了，因为在测试阶段进行预测时，我们不期望输出结果是随机的，如果测试阶段应用dropout函数，预测会受到干扰。
