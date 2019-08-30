---
title: MySQL事务以及事务的隔离级别
copyright: true
comments: true
date: 2019-08-30 16:14:49
tags:
 - MySQL
 - MySQL事务
 - MySQL隔离级别
categories: MySQL
description: 本文小编主要介绍一下MySQL中的事务以及相关事务的隔离级别总结，希望能帮到大家。
password:
sticky:
---
# 事务概述：
&emsp;&emsp;事务是由单独单元的一个或多个SQL语句组成，在这个单元中，每个MySQL语句是相互依赖的。而整个单独单元作为一个不可分割的整体，如果单元中某条SQL语句一旦执行失败或产生错误，那么整个单元将会回滚。所有受影响的数据将返回到事务开始以前的状态；如果单元中的所有SQL语句均执行成功，则事务被顺利执行。

# MySQL中的存储引擎
&emsp;&emsp;提到了事务，我们就先说说MySQL中的存储引擎。在MySQL中的数据用各种不同的技术存储在文件（或内存）中。

 - 查询MySQL中支持的存储引擎,通过如下命令：
 ```sql
 show engines;
 ```
&emsp;&emsp;在MySQL中用的最多的存储引擎有：InnoDB、Myisam、Memory等。其中 InnoDB 支持事务，而 Myisam、Memory等不支持事务。

# 事务的特点：
 - 事务的ACID(acid)属性
  - 原子性（Atomicity）
   - 原子性是指事务是一个不可分割的工作单位，事务中的操作要么都发生，要么都不发生。
  - 一致性（Consistency）
   - 事务必须使数据库从一个一致性状态变换到另外一个一致性状态。
  - 隔离性（Isolation）
   - 事务的隔离性是指一个事务的执行不能被其他事务干扰，即一个事务内部的操作及使用的数据对并发的其他事务时隔离的，并发执行的各个事务之间不能互相干扰。
  - 持久性（Durability）
   - 持久性是指一个事务一旦被提交，它对数据库中的数据的改变就是永久性的，接下来的其他操作和数据库故障不应该对其有任何影响。

# MySQL事务的分类

 - 隐式事务：事务没有明显的开启和结束的标记。比如：insert、update、delete 语句。
 - 显式事务：事务具有明显的开启和结束的标记。前提：必须先设置自动提交功能为禁用。

# 事务的创建

 - 步骤1：开启事务
 ```sql
 set autocommit = 0;
 start transaction;
 ```
 - 步骤2：编写事务中的sql语句（select、insert、update、delete）
 语句1；
 语句2；
 ...

 - 步骤3：结束事务
 ```sql
 commit; --提交事务
 rollback; --回滚事务
 ```
# 数据库的隔离级别

 - 对于同时运行的多个事务，当这些事务访问`数据库中相同的数据`时，如果没有采取必要的事务隔离机制，就会导致各种并发问题：脏读、不可重复读、幻读。

 - **数据库事务的隔离性：** 数据库系统必须具有隔离并发运行各个事务的能力，使它们不会互相影响，避免各种并发问题的出现。
 - `一个事务与其他事务隔离的程度称为隔离级别。`数据库规定了多种事务隔离级别，不同隔离级别对应不同的干扰程度，隔离级别越高，数据一致性就越好，但并发性就越弱。

 - **注意：**这里关于MySQL的事务隔离级别小编另外单独写了一篇博客：[MySQL的事务隔离级别]()。

 案例：
 ```sql
 SET autocommit=0;
 START TRANSACTION;

 UPDATE USER SET money = 1000 WHERE username = "张三";
 UPDATE USER SET money = 1000 WHERE username = "李四";

 --commit;
 ROLLBACK;
 ```
 结果：
 ![01.png](01.png)




