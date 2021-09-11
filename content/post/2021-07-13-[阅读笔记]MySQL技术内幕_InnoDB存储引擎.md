---
title: "[阅读笔记]MySQL技术内幕_InnoDB存储引擎"
date: 2021-07-13T20:47:09+08:00
draft: true

toc: true
categories: [Notes]
tags: [MySQL]
keywords: []
description: ""
---



# MySQL技术内幕_InnoDB存储引擎

[TOC]

## ch01 MySQL体系结构和存储引擎



## ch02 InnoDB存储引擎



## ch03 文件



## ch04 表



## ch05  索引与算法



## ch06 锁

- 什么是锁？

  - 锁 是数据库系统区别于文件系统的一个关键特性。锁机制用于管理对**共享资源**的并发访问。
  - InnoDB存储引擎会**在行级别**上对表数据上锁。
  - 数据库系统使用锁是为了支持对共享资源进行并发访问。
  - 有多少种数据库 ，可能就有多少种锁的实现方法。
  - InnoDB存储引擎锁的实现和Oracle数据库非常类似，提供一致性的非锁定读、行级锁支持。行级锁没有相关额外的开销，并可以同时得到并发性和一致性。

- lock 与 latch

  - **latch** 一般称为 闩锁（轻量级的锁），其要求锁定的时间必须非常短。若持续的时间长，则应用的性能非常差。在InnoDB存储引擎中，latch可以分为mutex（互斥量）和rwlock（读写锁）。其目的是用来保证并发线程操作临界资源的正确性，并且通常没有死锁检测的机制。

  - lock的对象是 <u>事务</u>，用来锁定的是数据库中的对象，如表、页、行。一般lock的对象仅在事务commit或rollback后进行释放（不同事务隔离级别释放的时间可能不同）。 lock正如在大多数数据库中一样，是有死锁机制的。

    ![lock与latch的比较](https://img.fzhiy.net/img/image-20210713011544851.png)

  - InnoDB存储引擎中的latch，可以通过SHOW ENGINE INNODB MUTEX进行查看

    ![SHOW ENGINE INNODB MUTEX输出结果说明](https://img.fzhiy.net/img/image-20210713011713001.png)

- InnoDB存储引擎中的锁

  - InnoDB存储引擎行级锁的类型：1）**共享锁（S Lock）**，允许读一行数据；2）**排他锁（X Lock）**，允许事务删除或更新一行数据。

  - <u>**锁兼容**</u>：如果一个事务T1已经获得行r的共享锁，那么另外的事务T2可以立即获得行r 的共享锁（读取没有改变行r的数据）；但若有其他事务T3想要获得行r的排他锁，则必须等待事务T1、T2释放行r上的共享锁——<u>锁不兼容</u>

    ![排他锁与共享锁的兼容性](https://img.fzhiy.net/img/image-20210713012127024.png)

    S 和 X锁都是行锁，兼容是指对同一记录（row）锁的兼容性。

    InnoDB存储引擎支持多粒度（granular）锁定，这种锁定允许事务在行级上的锁和表级上的锁同时存在。 

  - 意向锁（InnoDB存储引擎支持）：将锁定的对象分为多个层次，意向锁意味着 事务希望在更细粒度（fine granularity）上进行加锁。设计的目的主要是为了在一个事务中揭示下一行被请求的锁类型。支持两种意向锁：1）意向共享锁（IS Lock），事务想要获得一张表中某几行的共享锁；2）意向排他锁（IX Lock），事务想要获得一张表中某几行的排他锁。

    ![InnoDB存储引擎中锁的兼容性](https://img.fzhiy.net/img/image-20210713012739345.png)

  - <u>**InnoDB 1.0开始**</u>，在INFORMATION_SCHEMA架构下**添加了表INNODB_TRX、INNODB_LOCKS、INNODB_LOCK_WAITS**，通过这三张表，用户可以更简单的监控当前事务并分析可能存在的锁问题。

    ![image-20210713013018648](https://img.fzhiy.net/img/image-20210713013018648.png)

    ![image-20210713013048470](https://img.fzhiy.net/img/image-20210713013048470.png)

    lock_data并非是一个“可信”的值。

    ![image-20210713013129732](https://img.fzhiy.net/img/image-20210713013129732.png)

    > 此处的 阻塞的锁的ID对应的字段改为 blocking_lock_id。

  - 一致性非锁定读（consistent nonlocking read）：InnoDB存储引擎通过<u>行多版本控制</u>的方法来读取<u>当前执行时间</u>数据库中行的数据。

    ![image-20210713013455282](https://img.fzhiy.net/img/image-20210713013455282.png)

    快照数据是 该行的之前版本的数据，该实现是通过 undo 段来完成。undo 用来在事务中回滚数据，因此快照数据本身是没有额外的开销。 读取快照数据是不需要上锁的（没有事务需要对历史的数据进行修改操作）。

    **非锁定读机制极大的提高了数据库的并发性。**

    就上图所显示的，一个行记录可能有不止一个快照数据，一般称为 <u>行多版本技术</u>。由此带来的并发控制，称之为 **<u>多版本并发控制（MVCC，Multi Version Concurrency Control）</u>**。

  - 在事务隔离级别READ COMMITTED和REPEATABLE READ（InnoDB 默认）下，InnoDB存储引擎使用 非锁定的一致性读。在READ COMMITTED事务隔离级别下，对于快照数据，非一致性读 总是读取 <u>被锁定行的最新一份</u>快照数据；在REPEATABLE READ事务隔离级别下，对于快照数据，非一致性读 总是读取 <u>事务开始时的行版本数据</u>。

- 锁的算法

- 锁问题

- 阻塞

- 死锁

- 锁升级



## ch07 事务



## ch08 备份与恢复



# 参考文献

- MySQL技术内幕_InnoDB存储引擎 第二版