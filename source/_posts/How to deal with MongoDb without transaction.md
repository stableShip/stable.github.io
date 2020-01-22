---

title: How to deal with MongoDb without transaction

date: 2019-02-15 10:19:32

tags: [MongoDb, transaction]

---

## 什么是事务
请先了解事务相关的概念。[事务](https://baike.baidu.com/item/%E4%BA%8B%E5%8A%A1/5945882)

Please learn about transaction first.[Database_transaction](https://en.wikipedia.org/wiki/Database_transaction)

  
## Mongodb的事务
MongoDb 4.0版本之前，没有事务。 如何在没有事务的情况下，使用mongoDb？

There is no transaction in MongoDb before version 4.0. How to deal with it?

  

MongoDb中，一个写操作在一个document上是原子的。即使是操作这个document的嵌套的子document。

因为没有事务，当你更新多个文档（documents）的时候，一些错误发生了，mongoDb不会进行rollback处理。 或者是多个操作时，个个操作会有交错，导致数据更新错误（如：多个操作更新用户资产）。

In MongoDb, a write operation is atomic on the level of a single document, even if the operation modifies multiple embedded documents _within_ a single document.

Because there is no transaction， when you need to update  multiple documents. if there is something error, it won't rollback by itself。


## 如何处理这些情况呢？

<!-- more -->

###  更新mongoDb到4.*版本

  

###  模拟关系型数据库 transaction-- 二阶段提交

参考： [https://docs.mongodb.com/v3.4/tutorial/perform-two-phase-commits](https://docs.mongodb.com/v3.4/tutorial/perform-two-phase-commits/)

在每次操作时，模拟数据库记录transaction记录（status: 'Pedding'），每一次数据库操作产生一条记录transaction，父操作包含多个子操作。失败时，在对应操作的编写回滚逻辑.

缺点：不是数据库自带事务，在数据库层发生错误（当机，网络连接问题）。程序回滚逻辑会被中断。 数据错乱， 需要配置一系列的定时任务保证数据正确性。

  

### 优化数据结构

将多余的数据更新去除，保留在一个document中

例如：

用户资产，我们使用一个 assets 表保存了用户所有的资产操作，每一次资产操作记录一条记录。 当用户查询资产时， 遍历用户所有资产操作记录，得到用户现有资产。

  
----------------
问题：当用户资产操作过多，导致查询速度过慢，无法满足性能要求。


优化：
在用户User表中，添加了一个用户资产列，使用分布式锁，每次只能有一个进行用户资产操作，每次用户资产操作后，更新到User表中， 直接查询User表， 满足了性能要求。



-------------

问题：当数据库发生故障（当机），因为没有事务，User表中的资产列更新错误，或者没有更新到。导致用户查询错误。重大问题。



临时优化： 每天定时遍历所有用户资产，保证用户资产正确。

优化：将User表中的资产列， 迁移至assets表， 每一次添加用户操作时，记录当前最新的用户资产到该列， 没有多个document操作。确保操作原子性。