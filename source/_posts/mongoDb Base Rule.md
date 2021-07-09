---

title: MongoDb Base Rule
date: 2019-01-29 20:19:32
tags: [MongoDb]
categories: [MongoDb]

---

### Select 只获取必要的字段

why:

_select *会增加cpu/io/内存/带宽的消耗_

_指定字段能有效利用索引覆盖：find（{id： “test”}）.select({id: 1}) 命中索引id， 因只select了id， 所以mongo直接返回了id， 不会再索引到具体的document，效率大大提高。_

### 避免过度使用嵌入文档，最多只能一层， 不使用数组嵌套

why:

_document有16MB限制。_

  

### 尽量避免使用负向查询以及%开头的模糊查询

why:

_ne、nin %开头 无法有效使用索引_

### 禁止使用look up

why:

_分片后，look up操作失效_

### 不在数据库中进行数据计算

why:

_js精度问题 （0.1 + 0.2 = 0.30000000000000004__）_

_cpu计算导致数据库卡顿，解放数据库CPU，把复杂逻辑计算放到服务层。_

  

### 平衡范式与冗余

为提高效率，可以冗余数据

  

### 拒绝大sql，大批量 操作

  

### 禁止在更新十分频繁、区分度不高的属性上建立索引

  

why：

  

_更新会变更B+树，更新频繁的字段建立索引会大大降低数据库性能_

_“性别”这种区分度不大的属性，建立索引是没有什么意义的，不能有效过滤数据，性能与全表扫描类似_

_索引内存也是有限的_

  

### 使用explain 优化查询

_查看explain结果,_

_可以简单查看inputStages.stage判断查询效率。_

_COLLECTION：全表查询_

_IXSCAN：命中索引查询_

  

### 禁止使用自带全文索引

why:

_效率低_

  

  

  

  

  

[MongoDb_Demo](https://github.com/stableShip/Spring_MongoDb)

  

  

  

参考： [mysql 50条军规](http://blog.51cto.com/lee90/2096461)