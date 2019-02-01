---
title: pomelo服务器部署文档
date: 2015-04-23 11:07:43
categories: [node,pomelo]
tags: [pomelo,服务器]
---

# 运行环境
* [nodejs](http://nodejs.org/)
* Windows、Linux 或 MacOS 操作系统
* MySql 数据库

# 部署服务端

## 安装依赖包 

进入目录：
`cd xxx_server`

安装依赖包：
`sh npm-install.sh`(Windows: `npm-install.bat`)

# 创建MySql数据库

## 创建数据库
sql文件路径：./game-server/scripts/mysql.sql

* 安装MySql数据库(略)
* 登录MySql:
`mysql –u用户名 –p密码`
(登录成功提示符：mysql>)
* 创建数据库:
`mysql> create database dev;`
* 选择数据库:
`mysql> use dev;`
* 导入sql文件:
`mysql> source ./game-server/scripts/mysql.sql`


 <!-- more -->
## 修改数据库配置
数据库配置文件为./shared/mysql.json
```json
{
    "development": {
        "host": "127.0.0.1",
        "port": "3306",
        "database": "dev",
        "user": "user",
        "password": "password"
    },

    "production": {
        "host": "****",
        "port": "***",
        "database": "***",
        "user": "user",
        "password": "password"
    }
}
```

将"development"环境下的的数据库配置修改为实际的配置. 

# 运行游戏
需要分别启动game-server和web-server. 
game-server的启动方式：

* `pomelo start` (pomelo的安装方法参考[pomelo快速使用指南](https://github.com/NetEase/pomelo/wiki/pomelo快速使用指南)) 注: 如果上次启动的进程没有完全退出, 可以使用`pomelo kill --force`来结束所有node进程. 

web-server的启动方式：

* `cd web-server && node app.js`

# 访问游戏
客户端配置好之后，使用客户端给的url启动游戏

浏览器需支持websocket, 推荐使用chrome. 

# 相关问题解决办法
1. 端口冲突

修改服务器配置文件./game-server/config/servers.json,内容如下：
```json
{
    "development": {
        "connector": [{
            "id": "connector-server-1",
            "host": "120.24.161.192",
            "port": 4050,
            "clientPort": 3050,
            "frontend": true,
            "httpServerPort": 4020
        }],
        "auth": [{
            "id": "auth-server-1",
            "host": "192.168.1.169",
            "port": 5010,
            "clientPort": 3060,
            "frontend": true
        }],
        "hall": [{
            "id": "hall-server-1",
            "host": "192.168.1.169",
            "port": 6010,
            "hallType": "hallType1"
        }, {
            "id": "hall-server-2",
            "host": "192.168.1.169",
            "port": 6020,
            "hallType": "hallType2"
        }, {
            "id": "hall-server-3",
            "host": "192.168.1.169",
            "port": 6030,
            "hallType": "hallType3"
        }, {
            "id": "hall-server-4",
            "host": "192.168.1.169",
            "port": 6040,
            "hallType": "hallType4"
        }]
    },
    "production": {
       //...
    }
}
```

该配置文件分别定义了development和production环境下的各个服务器的配置信息, 包括服务器类型, 地址, 端口号等, production环境下的参数和development环境的结构类似. frontend参数为true时表示前端服务器. 端口冲突时可以修改相应的端口号. 



# 配置文件汇总说明
* ./game-server/config/master.json

master服务器的配置信息, 包括development和production环境下的服务器地址、端口号等. 

master服务器负责启动、关闭各服务器, 并监控所有服务器的状态信息. 
* ./game-server/config/servers.json

connector等服务器的配置信息, 包括development和production环境下的服务器地址、端口号等. 由于connector是前端服务器, 用于接收并转发玩家的请求, 所以会有clientPort. 

* ./scripts/mysql.json

数据库配置信息, 在部署服务端之后需要根据数据库安装的实际情况修改development和production环境的参数. 



