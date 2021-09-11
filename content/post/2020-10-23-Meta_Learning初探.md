---
title: "Meta Learning初探"
date: 2020-10-23T20:47:09+08:00
draft: false

toc: true
categories: [Notes]
tags: [Meta Learning]
keywords: []
description: ""
---


<span># 写在前面

> 本文主要记录Few-Shot Learning、Meta Learning相关的一些重要概念或小结。更多细节内容后续再补充。

文中部分图片来自CVPR 小样本PPT。

链接: <a href="https://pan.baidu.com/s/1Ya4hv3bqvcmxNPJVE0xLRQ#2020" target="_blank">https://pan.baidu.com/s/1Ya4hv3bqvcmxNPJVE0xLRQ</a> 提取码: 2020 复制这段内容后打开百度网盘手机App，操作更方便哦

# Few-shot Learning VS Meta-learning

![](https://img.fzhiy.net/img/20201023085718.png)

小样本学习：任何以有限数据很好地迁移为目标的迁移学习方法。如 预训练+微调，度量学习、元学习。

元学习：**学习** 学习算法本身，是许多小样本算法的组成部分（即应用于小样本学习中），也应用于多任务学习、强化学习中。

# 元学习整体流程

- 元学习训练meta learning :training time

  ![](https://img.fzhiy.net/img/20201023092537.png)

- 元学习测试meta learning: test time

  ![](https://img.fzhiy.net/img/20201023092739.png)

  

# 监督学习到元学习

![](https://img.fzhiy.net/img/20201023092423.png)

| 监督学习                                | 元学习                                            |
| --------------------------------------- | ------------------------------------------------- |
| 训练和测试过程分别为training和test time | 训练和测试过程分别为meta-training和meta-test time |
| 只在一个任务上做训练                    | 面向多个任务做联合训练                            |
| 只有一个训练集和一个测试集              | 每个任务都有训练集和测试集                        |
| 要求训练数据和测试数据满足独立同分布    | 要求新任务与训练任务在分布上尽可能一致            |
| 学习到的是样本之间的泛化能力            | 学习到的是任务之间的泛化能力                      |

# 元学习重点小结

- 小样本学习：任何以**少量数据**很好地迁移为目标的**迁移学习方法**。如 预训练+微调，度量学习、元学习；
- 元学习：**学习** 学习算法本身，是许多小样本算法的组成部分（即**应用于小样本学习中(也即元学习是小样本学习的一种？)**），也应用于多任务学习、强化学习中；元学习适用于 **小样本、多任务** 的学习场景，可解决在新任务缺乏训练样本的情况下 **快速学习 和 快速适应** 的问题；

- 元学习是通过优化loss值来拟合query examples，使得模型逐渐适应小样本的学习方式；
- 从参数优化的角度来说，元学习模型学习的是一个通用的初始化参数，当其碰到新任务时，通过训练能够快速收敛（**得到最优解？？这里感觉不一定，见第二条源自《百面深度学习》**）
- ![](https://img.fzhiy.net/img/20201023100157.png)
  - Learning to learn[M]. Springer Science & Business Media, 2012.

# 参考文献

- 《百面深度学习》
- [Meta-Learning: Learning to Learn Fast](https://lilianweng.github.io/lil-log/2018/11/30/meta-learning.html)

- [元学习：学习如何学习【译】](https://wei-tianhao.github.io/blog/2019/09/17/meta-learning.html)
- [小样本学习方法(FSL)演变过程](https://zhuanlan.zhihu.com/p/149983811)</span>