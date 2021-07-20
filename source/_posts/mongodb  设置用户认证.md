---
title: Mongodb3.* 设置用户认证
date: 2016-03-27 20:07:43
tags: [MongoDb]
categories: [MongoDb]
---

# mongodb3.* 设置用户认证

1、mongodb是没有默认管理员账号，所以要先添加管理员账号，在开启权限认证。
2、切换到admin数据库，添加的账号才是管理员账号。
3、用户只能在用户所在数据库登录，包括管理员账号。
4、管理员可以管理所有数据库，但是不能直接管理其他数据库，要先在admin数据库认证后才可以。这一点比较怪

## 添加管理员账号：
默认情况下系统中没有用户,正常启动mongo:`mongod`
<!-- more -->

```
添加管理员账号：
> use admin    --切换到admin库添加管理员用户
switched to db admin

> db.createUser({user:"root",pwd:"123456",roles:["root"]})
Successfully added user: { "user" : "root", "roles" : [ "root" ] }

> db.auth("root","123456")   --验证
1

添加普通账号：
> use yhtsdk    --切换到test库添加普通用户
switched to db yhtsdk


> db.createUser({user:"root",pwd:"123456",roles:["dbOwner"]})
Successfully added user: { "user" : "root", "roles" : [ "dbOwner" ] }

> db.auth("root","123456")   --验证
1


> show user;
{"_id" : "yhtsdk.root",	"user" : "root","db" : "sf_db",	"roles" : [{"role" :dbOwner",
"db" : "sf_db"}]}

> use admin;
> db.system.users.find(); --查询添加的用户
{ "_id" : "yhtsdk.root", "user" : "root", "db" : "yhtsdk", "credentials" : { "SCRAM-SHA-1" : { "iterationCount" : 10000, "salt" : "fellOtPg/uWxKrzd/SXzhQ==", "storedKey" : "zIf2Qcio2FD0utBt20UJX2pwCoc=", "serverKey" : "MMMywkCX9WfVgiHi8P04P4PMQ0w=" } }, "roles" : [ { "role" : "dbOwner", "db" : "yhtsdk" } ] }

测试
`mongo 127.0.0.1/yhtsdk -u root -p 123456`
MongoDB shell version: 3.2.5
connecting to: yhtsdk
> 



删除账户:
> use yhtsdk    --切换到要删除用户的数据库
> db.system.users.remove({"user":"test"})

修改用户密码:
> db.changeUserPassword('root','test'); 
```

### 参考:
http://www.jb51.net/article/50546.htm


### role:

> Built-In Roles（内置角色）：
    1. 数据库用户角色：read、readWrite;
    2. 数据库管理角色：dbAdmin、dbOwner、userAdmin；
    3. 集群管理角色：clusterAdmin、clusterManager、clusterMonitor、hostManager；
    4. 备份恢复角色：backup、restore；
    5. 所有数据库角色：readAnyDatabase、readWriteAnyDatabase、userAdminAnyDatabase、dbAdminAnyDatabase
    6. 超级用户角色：root  
    // 这里还有几个角色间接或直接提供了系统超级用户的访问（dbOwner 、userAdmin、userAdminAnyDatabase）
    7. 内部角色：__system