---
layout: post
title:  "归一化网络的激活函数（Normalizing activations in a network）-吴恩达 深度学习 course2 3.4~3.7笔记"
date:   2018-05-12 19:51:31
categories: DeepLearning
tags: DeepLearning 吴恩达
mathjax: true
---
* content
{:toc}
Batch归一化（Batch Normalization，经常简称为 BN）会使你的参数搜索问题变得很容易，使神经网络对超参数的选择更加稳定，超参数的范围会更加庞大，工作效果也很好，也会是你的训练更加容易，甚至是深层网络。
<!--more-->

# 3.4 归一化网络的激活函数（Normalizing activations in a network）
在深度学习兴起后，最重要的一个思想是它的一种算法，叫做Batch归一化。

**Batch归一化**（Batch Normalization，经常简称为 BN）会使你的参数搜索问题变得很容易，使神经网络对超参数的选择更加稳定，超参数的范围会更加庞大，工作效果也很好，也会是你的训练更加容易，甚至是深层网络。

之前，我们对输入特征 X 使用了标准化处理。我们也可以用同样的思路处理隐藏层的激活值 a[l]，以加速 W[l+1]和 b[l+1]的训练。在实践中，经常选择标准化 Z[l]：

$$
\mu = \frac{1}{m} \sum_i z^{(i)}
$$

$$
\sigma^2 = \frac{1}{m} \sum_i {(z_i - \mu)}^2
$$

$$
z_{norm}^{(i)} = \frac{z^{(i)} - \mu}{\sqrt{\sigma^2 + \epsilon}}
$$

其中，m 是单个 mini-batch 所包含的样本个数，ϵ 是为了防止分母为零，通常取 10−8。

这样，我们使得所有的输入 z(i)均值为 0，方差为 1。但我们不想让隐藏层单元总是含有平均值 0 和方差 1，也许隐藏层单元有了不同的分布会更有意义。因此，我们计算

$$
\tilde z^{(i)} = \gamma z^{(i)}_{norm} + \beta
$$

其中，γ 和 β 都是模型的学习参数，所以可以用各种梯度下降算法来更新 γ 和 β 的值，如同更新神经网络的权重一样。

![batch normalization](http://p5ocy6pck.bkt.clouddn.com/Batch%20Normalization.png)

通过对 γ 和 β 的合理设置，可以让
$$
\tilde z^{(i)}
$$
的均值和方差为任意值。这样，我们对隐藏层的 z^(i)进行标准化处理，用得到的
$$
\tilde z^{(i)}
$$
替代 z(i)。

设置 γ 和 β 的原因是，如果各隐藏层的输入均值在靠近 0 的区域，即处于激活函数的线性区域，不利于训练非线性神经网络，从而得到效果较差的模型。因此，需要用 γ 和 β 对标准化后的结果做进一步处理。

# 3.5 将 Batch Norm 拟合进神经网络（Fitting Batch Norm into a neural network）
对于 L 层神经网络，经过 Batch Normalization 的作用，整体流程如下：

![adding batch norm to a network](http://p5ocy6pck.bkt.clouddn.com/adding%20batch%20norm%20to%20a%20network.png)

实际上，Batch Normalization 经常使用在 mini-batch 上，这也是其名称的由来。

使用 Batch Normalization 时，因为标准化处理中包含减去均值的一步，因此 b 实际上没有起到作用，其数值效果交由 β 来实现。因此，在 Batch Normalization 中，可以省略 b 或者暂时设置为 0。

在使用梯度下降算法时，分别对 W[l]，β[l]和 γ[l]进行迭代更新。除了传统的梯度下降算法之外，还可以使用之前学过的动量梯度下降、RMSProp 或者 Adam 等优化算法。

# 3.6 Batch Norm 为什么奏效？（Why does Batch Norm work?）

Batch Normalization 效果很好的原因有以下两点：

- 通过对隐藏层各神经元的输入做类似的标准化处理，提高神经网络训练速度；
- 可以使前面层的权重变化对后面层造成的影响减小，整体网络更加健壮。

关于第二点，如果实际应用样本和训练样本的数据分布不同（例如，橘猫图片和黑猫图片），我们称发生了“**Covariate Shift**”。这种情况下，一般要对模型进行重新训练。Batch Normalization 的作用就是减小 Covariate Shift 所带来的影响，让模型变得更加健壮，鲁棒性（Robustness）更强。

即使输入的值改变了，由于 Batch Normalization 的作用，使得均值和方差保持不变（由 γ 和 β 决定），限制了在前层的参数更新对数值分布的影响程度，因此后层的学习变得更容易一些。Batch Normalization 减少了各层 W 和 b 之间的耦合性，让各层更加独立，实现自我训练学习的效果。

另外，**Batch Normalization 也起到微弱的正则化（regularization）效果**。因为在每个 mini-batch 而非整个数据集上计算均值和方差，只由这一小部分数据估计得出的均值和方差会有一些噪声，因此最终计算出的 z~(i)也有一定噪声。类似于 dropout，这种噪声会使得神经元不会再特别依赖于任何一个输入特征。

因为 Batch Normalization 只有微弱的正则化效果，因此可以和 dropout 一起使用，以获得更强大的正则化效果。通过应用更大的 mini-batch 大小，可以减少噪声，从而减少这种正则化效果。

最后，不要将 Batch Normalization 作为正则化的手段，而是当作加速学习的方式。正则化只是一种非期望的副作用，Batch Normalization 解决的还是反向传播过程中的梯度问题（梯度消失和爆炸）。

# 3.7 测试时的 Batch Norm（Batch Norm at test time）
在训练时， μ 和 σ2是在整个mini-batch上计算出来的.包含了像是64或28或其它一定数量的样本

但在测试时，你可能需要逐一处理样本，方法是根据你的训练集估算和，估算的方式有很多种，理论上你可以在最终的网络中运行整个训练集来得到 μ 和 σ2

但在实际操作中，我们通常运用指数加权平均来追踪在训练过程中你看到的 μ 和 σ2的值。还可以用指数加权平均，有时也叫做流动平均来粗略估算 μ 和 σ2，然后在测试中使用和的值来进行你所需要的隐藏单元值的调整。

对于第 l 层隐藏层，考虑所有 mini-batch 在该隐藏层下的 μ[l]和 σ2[l]，然后用指数加权平均的方式来预测得到当前单个样本的 μ[l]和 σ2[l]。这样就实现了对测试过程单个样本的均值和方差估计。
