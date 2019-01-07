---
layout: post
title:  "梯度检验（Gradient checking）-吴恩达 深度学习 course2 1.13笔记"
date:   2018-04-19 20:35:10
categories: DeepLearning
tags: DeepLearning 吴恩达
mathjax: true
---

* content
{:toc}

梯度检验帮我们节省了很多时间，也多次帮我发现backprop实施过程中的bug，
<!--more-->



- 梯度检验帮我们节省了很多时间，也多次帮我发现backprop实施过程中的bug，接下来，我们看看如何利用它来调试或检验backprop的实施是否正确。

## 连接参数
![Gradient checking](http://p5ocy6pck.bkt.clouddn.com/Gradient%20checking.png)

将 W[1]，b[1]，...，W[L]，b[L]全部连接出来，成为一个巨型向量 θ。这样，


```
            J(W[1],b[1],...,W[L]，b[L])=J(θ)
```

同时，对 dW[1]，db[1]，...，dW[L]，db[L]执行同样的操作得到巨型向量 dθ，它和 θ 有同样的维度。

## 进行梯度检验

- 现在的问题是dθ 和代价函数J的梯度或坡度有什么关系？

![Gradient checking](http://p5ocy6pck.bkt.clouddn.com/Gradient%20checking2.png)

首先，我们要清楚 J 是超参数 θ 的一个函数。

使用双边误差，也就是


$$
d\theta_{approx}[i] ＝ \frac{J(\theta_1, \theta_2, ..., \theta_i+\varepsilon, ...) - J(\theta_1, \theta_2, ..., \theta_i-\varepsilon, ...)}{2\varepsilon}
$$


具体来说，如何定义两个向量是否真的接近彼此？我一般做下列运算，计算这两个向量的距离


$$
\frac{||d\theta_{approx} - d\theta||_2}{||d\theta_{approx}||_2+||d\theta||_2}
$$

注意这里没有平方，它是误差平方之和，然后求平方根，得到欧式距离，然后用向量长度归一化，使用向量长度的欧几里得范数。分母只是用于预防这些向量太小或太大，分母使得这个方程式变成比率

如果左边这个方程式结果是10^-3 ,我就会担心是否存在bug，计算结果应该比10^-3小很多，如果比大很多，我就会很担心，担心是否存在bug。这时应该仔细检查所有项，看是否有一个具体的值，使得与dθ[i]大不相同，并用它来追踪一些求导计算是否正确，经过一些调试，最终结果会是这种非常小的值（10^-7），那么，你的实施可能是正确的。

- 在实施神经网络时，我经常需要执行foreprop和backprop，然后我可能发现这个梯度检验有一个相对较大的值，我会怀疑存在bug，然后开始调试，调试，调试，调试一段时间后，我得到一个很小的梯度检验值，现在我可以很自信的说，神经网络实施是正确的。
