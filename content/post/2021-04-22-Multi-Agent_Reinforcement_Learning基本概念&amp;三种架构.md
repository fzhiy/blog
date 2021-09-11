---
title: "Multi-Agent Reinforcement Learning基本概念&三种架构"
date: 2021-02-22T20:47:09+08:00
draft: false

toc: true
categories: [Notes]
tags: [Multi-Agent Reinforcement Learning]
keywords: []
description: ""
---


> 参考内容在References写出，仅作为个人学习笔记，如有错误欢迎指出。
>
> References的一本偏向数学推理的DRL新书即将上线？安排上

# Multi-Agent Reinforcement Learning基本概念(1/2)

## Settings

1. Fully cooperative 相互配合 e.g. 工业机器人

2. Fully competitive 一方的收益是另一方的损失（捕猎者和猎物），如 **零和博弈**，双方获得的奖励总和等于0

3. Mixed Cooperative % competitive   e.g. 足球机器人 同队伍的机器人与不同队伍的机器人分别是合作与竞争关系

4. Self-interested  利己主义，即 **每个agent只想要最大化自身利益** 如 股票和期货的自动交易系统

   ![](https://img.fzhiy.net/img/20210422192256.png)

   ![](https://img.fzhiy.net/img/20210422192421.png)

## Terminologies

- State，Action，State Transition （**agent之间会相互影响，而非独立**）

  ![](https://img.fzhiy.net/img/20210422192631.png)

- Rewards

  ![](https://img.fzhiy.net/img/20210422192804.png)

- Returns

  ![](https://img.fzhiy.net/img/20210422192935.png)

- Policy Network

  ![](https://img.fzhiy.net/img/20210422193114.png)

- Uncertainty in the Return

  ![](https://img.fzhiy.net/img/20210422193226.png)

- State-Value Function

  ![](https://img.fzhiy.net/img/20210422193534.png)

  动作是随机的，与$\theta^j$相关，因此$V^i$**依赖于所有的$\theta^1,...,\theta^n$**

  ![](https://img.fzhiy.net/img/20210422193812.png)

## Convergence

- Single-Agent Policy Learning

  ![](https://img.fzhiy.net/img/20210422193933.png)

- Multi-Agent Policy Learning

  Nash Equilibrium 纳什均衡

  ![](https://img.fzhiy.net/img/20210422194108.png)

- Difficulty of MARL

  Single-Agent Policy Gradient for MARL

  ![](https://img.fzhiy.net/img/20210422194209.png)

  ![](https://img.fzhiy.net/img/20210422194341.png)

  ![](https://img.fzhiy.net/img/20210422194441.png)

  ![](https://img.fzhiy.net/img/20210422194550.png)

  用single-agent policy来做MARL的问题在于：当一个智能体达到最优策略时，另一个智能体继续调整$\theta$，其他的智能体的目标函数都会改变，因此他们也将改变 **自己的策略**。 即 **MARL的最优策略依赖于所有的$\theta^i,i=1,...,n$ **

## Summary

- MARL **只有所有的agents之间相互独立时，才能将single-agent RL方法用于MARL**

![](https://img.fzhiy.net/img/20210422194941.png)

- Setting of MARL 四种设置

- Convergence 

  ![](https://img.fzhiy.net/img/20210422195305.png)

  对于single-agent System，目标函数不再增长时 收敛；对于multi-agent System，纳什均衡 表示收敛

# Multi-Agent Reinforcement Learning三种架构(2/2)

## Architectures

- 完全去中心化 **agents之间不通信**
- 完全中心化 （可理解为 “定于一尊“） **中央控制器为所有的agents做决策**
- 中心化训练、去中心化执行 （训练过程使用中央控制器，训练后不使用中央控制器，**每个agent根据自己的观测用自己的策略网络做决策**）

![](https://img.fzhiy.net/img/20210422200818.png)

## Partial Observations （MARL的假设）

![](https://img.fzhiy.net/img/20210422201335.png)

## Fully Decentralized Training

![](https://img.fzhiy.net/img/20210422202358.png)

Single-Agent RL 架构图（来自基本概念章节）

![](https://img.fzhiy.net/img/20210422194209.png)

通过上面两张图发现 完全去中心化架构的**本质是Single-Agent RL** 而不是MARL

==Fully Decentralized Actor-Critic Method==

![](https://img.fzhiy.net/img/20210422202828.png)

## Fully Centralized 

**Centralized Training**

![](https://img.fzhiy.net/img/20210422203136.png)

**Centralized  Execution**

中心化执行的时候， 输入是$o^1,...,o^n$ ，策略由中央控制器来做，因为做决策需要知道所有的观测o， 一个agent只知道自己的观测o，而不知道其他的观测。

![](https://img.fzhiy.net/img/20210422203355.png)

**Centralized Actor-Critic Mehod**

![](https://img.fzhiy.net/img/20210422203504.png)

![](https://img.fzhiy.net/img/20210422203553.png)

中心化架构的好处是 中央知道所有的观测o 可以帮助更好的决策。缺点是**执行缓慢**，具体如下

### Shortcomming：Slow during Execution

![](https://img.fzhiy.net/img/20210422203747.png)

## Centralized Training with Decentralized Execution

![](https://img.fzhiy.net/img/20210422203935.png)

**Centralized Training**

![](https://img.fzhiy.net/img/20210422204002.png)

![](https://img.fzhiy.net/img/20210422204038.png)

![](https://img.fzhiy.net/img/20210422204114.png)

**Decentralized Execution**

![](https://img.fzhiy.net/img/20210422204144.png)

## Parameter Sharing？

![](https://img.fzhiy.net/img/20210422204215.png)

网络之间是否要共享参数呢？

根据具体场景来，比如 足球机器人比赛，agents是不可交换的（理解为负责的任务不同），所以不能共享参数。无人车由同样的网络控制，所以参数可以共享。

## Summary

- Fully Decentralized

  ![](https://img.fzhiy.net/img/20210422204435.png)

- Fully Centralized

  ![](https://img.fzhiy.net/img/20210422204448.png)

- Centralized Training with Decentralized Execution

  ![](https://img.fzhiy.net/img/20210422204545.png)

### 三种架构的对比

![](https://img.fzhiy.net/img/20210422204702.png)



# References

- https://www.youtube.com/watch?v=0HV1hsjd1y8
- https://github.com/wangshusen/DRL/blob/master/Notes_CN/DRL.pdf





