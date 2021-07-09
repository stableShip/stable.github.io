---
title: 在tornado中使用异步mysql操作
date: 2015-07-03 19:07:43
categories: [Python]
tags: [Tornado,Mysql,异步]
---

>在使用tornado框架进行开发的过程中,发现tornado的mysql数据库操作并不是一步的,造成了所有用户行为的堵塞.tornado本身是一个异步的框架,要求所有的操作都应该是异步的,但是数据库这一层就把整个服务器都拖住了.

##查找到的解决办法:
1. 使用异步的mysql操作库.  查找了一下,有两个比较完善的异步操作库
一个是[AsyncTorndb](https://github.com/mayflaver/AsyncTorndb),国人自己写的异步操作,看了一下,好像不错的样子,但是没有响应的测试用例,不敢用.

	一个是[Tornado-MySQL](https://github.com/PyMySQL/Tornado-MySQL)是对PyMySQL的异步化的一个库,测试用例,文档,都比较齐全,可以尝试使用.

2.仿照(torngas)[https://github.com/mqingyn/torngas]的异步线程池,使用tornado的concurrent.run_on_executor装饰器对数据库操作进行异步化

3.使用任务队列,太过麻烦,对之前的代码修改过大,不使用该方案

* 在使用Tornado-MySQL过程中,发现对现有代码更改太过严重,放弃,使用了异步线程池的方式.做到最小的代码更改以及异步数据库操作的实现

##如何使用异步线程池[concurrent.run_on_executor](http://www.tornadoweb.org/en/stable/concurrent.html?highlight=run_on_executor#tornado.concurrent.run_on_executor)

1. 在原先的同步的数据库执行的方法添加@concurrent.run_on_executor装饰器,如以下例子:

	``` python
	    @concurrent.run_on_executor
	        def runSql(self):
	            t = time.time()
	            db = client.conn()
	            db.execute('''select * from TABLE_CONSTRAINTS join (CHARACTER_SETS,STATISTICS)''')
	            db.close()
	            return time.time() - t
	            
	```
<!-- more -->

2.  在调用该方法的函数使用yield tornado.gen.Task(functionName) 调用上面的修改的方法,并且为主函数添加@tornado.gen.engine装饰器,如以下例子(tordona框架中的requestHander中的get方法):

	``` python
	        @tornado.web.asynchronous
	        @tornado.gen.engine
	        def get(self, *args, **kwargs):
	            # print self.get_query_argument("test11")
	            time = yield tornado.gen.Task(self.runSql)
	            print time
	            self.write(unicode(time))
	            print "over"
	            self.finish()
	```


	*使用[@tornado.web.asynchronous](http://www.tornadoweb.org/en/stable/web.html?highlight=tornado.web.asynchronous#tornado.web.asynchronous) 装饰器取消requestHander的自动finish,不然无法等待异步sql执行完毕再返回数据


[demo地址](https://github.com/stableShip/tornado_async_mysql)








