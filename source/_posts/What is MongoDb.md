---

title: What is MongoDb?
date: 2019-01-30 20:19:32  
tags: [mongoDb]

---

MongoDb 非关系数据库 NoSql 数据库一员， 将数据使用灵活，类JSON形式进行存储。更多：[what-is-mongodb](https://www.mongodb.com/what-is-mongodb)

MongoDb is a member of the NoSql， it stores data in flexible, JSON-like documents.

  

## 概念 Concepts between SQL and MongoDb
|SQL| MongoDb |
|--|--|
| Table | Collection |
| Row| Document|
| Column| Field|
| JOINs| Embedded documents, $lookup & $graphLookup|
| GROUP_BY| Aggregation -〉group by|
  

## 操作 Operation between SQL and MongoDb




|SQL|MongoDb|
|--|--|
|INSERT INTO users (user_id, age, status) VALUES 'bcd001', 45, 'A')|db.users.insert({  user_id: 'bcd001',  age: 45,  status: 'A'})|
|SELECT * FROM users|db.users.find()|
|UPDATE users SET status = 'C' WHERE age > 25|db.users.update(  { age: { $gt: 25 } },  { $set: { tatus: 'C' } },  { multi: true })|
|db.start_transaction() cursor.execute(orderInsert, orderData) cursor.execute(stockUpdate, stockData)
db.commit()|s.start_transaction() orders.insert_one(order, session=s) stock.update_one(item, stockUpdate, session=s)s.commit_transaction()  （new feature in 4.0 version）|
  

### 最大区别 Biggest different between SQL and MongoDb

  |SQL|MongoDb|
  |--|--|
  |使用关系型数据库， 需要在数据库中定义对应的表和字段关联。当你需要修改到对应的字段， 必须对数据库进行停机修改，会影响现有的所有数据， 降低应用可用性。In SQL, you pre-define your database schema based on your requirements and set up rules to govern the relationships between fields in your tables.Any changes in schema necessitates a migration procedure that can take the database offline or significantly reduce application performance.|使用MongoDb， 字段 无需在数据库中进行定义，使用代码定义对应的关系。如果需要添加一个新的字段，无需影响其他的document，无需停机进行更新。In MongoDb， Fields is no need to declare the structure of documents to the system – documents are self-describing in Code。If a new field needs to be added to a document,then the field can be created without affecting all other documents in the collection,without updating a central system catalog, and without taking the system offline.|
|更多的控制在数据库（DBA）， 索引，字段 都需要在数据库进行定义处理More control in Db, Indexes. Filed all controlled in Db side.|更多的控制在代码（开发）， 开发可以在代码中定义对应的索引，字段结构。Less control in Db, more control in Code. Indexes, Fields can controlled in Code side.|

  

参考：[mongodb-mysql](https://www.mongodb.com/compare/mongodb-mysql)