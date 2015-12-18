---
title: pomelo启用pomelo-admin-web
date: 2015-05-17 19:07:43
categories: [node,pomelo]
tags: [node,pomelo,pomelo-admin-web]
---


# 运行环境
* [nodejs](http://nodejs.org/)
* Linux 或 MacOS 操作系统


# 部署

### 下载源码

`git clone https://github.com/NetEase/pomelo-admin-web.git`

### 安装依赖包 

进入目录：
`cd pomelo-admin-web`

安装依赖包：
`npm install `



 
## 修改服务器配置

 <!-- more -->

1. 进入pomelo实例项目[lordofpomelo](https://github.com/NetEase/lordofpomelo)：
下载`lordofpomelo/game-server/app/modules/ ` 下的`onlineUser,sceneInfo` 两个文件，
放到你自己的项目的`/game-server/app/modules/`下


2. 配置/game-server/app.js，引入刚刚下载的两个文件
> var sceneInfo = require('./app/modules/sceneInfo');
   var onlineUser = require('./app/modules/onlineUser');`

 

 在`app.configure('production|development,...)`中加入

	> app.enable('systemMonitor'); //开启系统监控，必须
	app.filter(pomelo.filters.time()); //开启conn日志，对应pomelo-admin模块下conn request
	app.rpcFilter(pomelo.rpcFilters.rpcLog());//开启rpc日志，对应pomelo-admin模块下rpc request
    if (typeof app.registerAdmin === 'function') {
        app.registerAdmin(sceneInfo, {
            app: app
        });
        app.registerAdmin(onlineUser, {
            app: app
        });
    }



# 运行

1. 分别启动game-server和web-server. 进入游戏，进行几下操作

2. 进入admin-web目录：
	`cd pomelo-admin-web && node app.js` 
	打开浏览器，打开`https://localhost:7001`进入admin-web管理界面



# 配置文件汇总说明
* ./game-server/config/adminUser.json

配置admin-web用户信息，对应admin-web中的config/admin.json文件
具体信息请看[pomelo-admin](https://github.com/NetEase/pomelo-admin)






