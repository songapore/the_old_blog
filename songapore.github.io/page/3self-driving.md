---
layout: page
title: self-driving
permalink: /self-driving/
icon: bookmark
type: page
---

* content
{:toc}

## 无人驾驶的应用

预计到2020年，无人车将进入市场，从此开启一个崭新的阶段。到2040年，无人车将占全球市场25%的份额。

研究表明，在**增强高速公路安全**、**缓解交通拥堵**、**减少空气污染**等领域，无人驾驶会带来颠覆性的改善。
<!--
更多介绍： [无人驾驶的应用](https://note.youdao.com/)
-->

## 自动驾驶的分级
![自动驾驶的分级比较](http://p5ocy6pck.bkt.clouddn.com/%E8%87%AA%E5%8A%A8%E9%A9%BE%E9%A9%B6%E7%9A%84%E5%88%86%E7%BA%A7%E6%AF%94%E8%BE%83.jpg)

其中，NHTSA是 美国国家公路交通安全管理局，SAE为美国机动工程师协会，两者的区别在于SAE对NHTSA的完全自动驾驶进行了细分，强调了行车对环境与道路的要求。

<!--
更多介绍： [自动驾驶的分级](https://note.youdao.com/)
## 推荐学习顺序

学习无人驾驶得有个步骤, 下面大家就能按照自己所需, 来探索这个网站. 图中请找到 "Start", 然后依次沿着箭头, 看看有没有不了解/没学过的地方, 接着, 就能在底下的链接中找到对应的链接去往合适自己的地方.
-->



## 无人驾驶简介

![无人驾驶系统架构图](http://p5ocy6pck.bkt.clouddn.com/%E6%97%A0%E4%BA%BA%E9%A9%BE%E9%A9%B6%E7%B3%BB%E7%BB%9F%E6%9E%B6%E6%9E%84%E5%9B%BEcn.jpg)

无人驾驶并不是单个的技术，而是多个技术的整合。无人驾驶的整体技术架构，可以分为三大模块：
- **算法端**：
- **Client端**：
- **云端**：

算法端从传感器原始数据中提取有意义的信息以了解周遭的环境，并根据环境变化做出决策。Client端融合多种算法以满足实时性与可靠性的要求。云端为无人车提供离线计算机存储功能。

### 算法端
算法系统由三部分组成：
- 传感；
- 感知：
- 决策：
#### 传感
一般一辆无人车装备许多不同类型的主传感器。每一种类型的传感器各自有不同的优缺点，因此，需要将不同传感器的数据进行有效的融合。现在无人车中普遍使用的传感器包括：
1. GPS/IMU
2. LIDAR
3. 摄像头
4. 雷达和声呐

#### 感知
在获得传感信息之后，数据将被推送至感知子系统以充分了解无人车所处的周遭环境。其中，感知子系统主要做三件事：
1. 定位
2. 物体识别
3. 追踪

#### 决策
在决策阶段，行为预测、路径规划及避障极致三者结合起来实时地完成无人驾驶的动作规划。
1. 行为预测
2. 路径规划
3. 避障

### Client端

- 软件平台：机器人操作系统

-  硬件平台

### 云端
- 云平台主要从 **分布式计算** 及 **分布式存储** 两方面对无人驾驶系统提供支持。

- 使用**Spark**构建分布式计算平台，使用**OpenCL**构建异构计算平台，使用**Alluxio**作为内存存储平台。