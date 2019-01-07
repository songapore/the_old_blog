---
layout: post
title:  " 深度学习框架（Deep Learning frameworks）-吴恩达 深度学习 course2 3.10笔记"
date:   2018-05-21 10:20:23
categories: DeepLearning
tags: DeepLearning 吴恩达
mathjax: true
---
* content
{:toc}

利用一些深度学习框架会更加实用，会使你的工作更加有效，那就让我们来看下有哪些框架
<!--more-->


# 3.10 深度学习框架（Deep Learning frameworks）

我认为现在深度学习已经很成熟了，利用一些深度学习框架会更加实用，会使你的工作更加有效，那就让我们来看下有哪些框架。

- Caffe / Caffe 2
- CNTK
- DL4J
- Keras
- Lasagne
- mxnet
- PaddlePaddle
- TensorFlow
- Theano
- Torch

## 选择框架的标准

- 便于编程：包括神经网络的开发和迭代、配置产品；
- 运行速度：特别是训练大型数据集时；
- 是否真正开放：不仅需要开源，而且需要良好的管理，能够持续开放所有功能。


# 3.11 TensorFlow

有很多很棒的深度学习编程框架，其中一个是TensorFlow

Tensorflow 框架内可以直接调用梯度下降算法，极大地降低了编程人员的工作量。例如

![tensorflow code example](http://p5ocy6pck.bkt.clouddn.com/tensorflow%20code%20example.png)

- Writing and running programs in TensorFlow has the following steps:

    - Create Tensors (variables) that are not yet executed/evaluated.
    - Write operations between those Tensors.
    - Initialize your Tensors.
    - Create a Session.
    - Run the Session. This will run the operations you'd written above.

```
import numpy as np
import tensorflow as tf

cofficients = np.array([[1.],[-20.],[25.]])

w = tf.Variable(0,dtype=tf.float32)

x = tf.placeholder(tf.float32,[3,1])
# cost = tf.add(tf.add(w**2,tf.multiply(- 10.,w)),25)
# Tensorflow 重载了加减乘除符号
cost = x[0][0]*w**2 + x[1][0]*w + x[2][0]

# 改变下面这行代码，可以换用更好的优化算法
train = tf.train.GradientDescentOptimizer(0.01).minimize(cost)
# 下面的几行是惯用表达式:
init = tf.global_variables_initializer()

session = tf.Session()#这样就开启了一个TensorFlow session。
session.run(init)#来初始化全局变量。
print(session.run(w))
# with tf.Session() as session:
#   session.run(init)
#   print(session.run(w))    

for i in range(1000):
    session.run(train, feed_dict=(x:coefficients))
print(session.run(w))

```
- 一旦被称为TensorFlow变量，平方，乘法和加减运算都重载了
- with 语句适用于对资源进行访问的场合，确保不管使用过程中是否发生异常都会执行必要的“清理”操作，释放资源，比如文件使用后自动关闭、线程中锁的自动获取和释放等。
- TensorFlow中的placeholder是一个你之后会赋值的变量，这种方式便于把训练数据加入损失方程
- 当你运行训练迭代，用feed_dict来让x=coefficients。如果你在做mini-batch梯度下降，在每次迭代时，你需要插入不同的mini-batch，那么每次迭代，你就用feed_dict来喂入训练集的不同子集，把不同的mini-batch喂入损失函数需要数据的地方。
