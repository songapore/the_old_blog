---
layout: post
title:  "梯度的数值逼近（Numerical approximation of gradients）-吴恩达 深度学习 course2 1.12笔记"
date:   2018-04-19 20:30:10
categories: DeepLearning
tags: DeepLearning 吴恩达
mathjax: true
---

* content
{:toc}

在实施backprop时，有一个测试叫做梯度检验，它的作用是确保backprop正确实施
<!--more-->



在实施backprop时，有一个测试叫做梯度检验，它的作用是确保backprop正确实施。因为有时候，你虽然写下了这些方程式，却不能100%确定，执行backprop的所有细节都是正确的。为了逐渐实现梯度检验，我们首先说说如何计算梯度的数值逼近

![image](http://p5ocy6pck.bkt.clouddn.com/one-and-two-sided-difference.png.png)

当 ε 越小时，结果越接近真实的导数，也就是梯度值。

我们可以使用这个方法来检验反向传播是否得以正确实施，如果不正确，它可能有bug需要你来解决。
