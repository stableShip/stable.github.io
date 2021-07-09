---
title: Pomelo启用第三方rpc库---pomelo-rpc-ws
date: 2015-06-08 19:07:43
categories: [Node,Pomelo]
tags: [Node,RPC,Pomelo,Pomelo-RPC-WS]
---


> 使用pomelo本身自带的rpc库,进行rpc通讯,发现连续多个rpc请求之后,会有一个延时相当严重得请求出现,没有找到相关的解决办法,最后使用`pomelo-rpc-ws`第三方rpc库解决了这个问题

# 运行环境
* [nodejs](http://nodejs.org/)
* Linux 或 MacOS 操作系统
* [pomelo-rpc-ws](https://github.com/skyblue/pomelo-rpc-ext)

##安装

`npm install pomelo-rpc-ws`





## 修改服务器配置

 

### 配置/game-server/app.js,在全局配置中加入下面的代码

```
// global configure for all servers
app.configure('production|development', function() {
	//启用pomelo-rpc-ws
    app.set('proxyConfig', {
        rpcClient: wsrpc.client
    });

    app.set('remoteConfig', {
        rpcServer: wsrpc.server
    });
});
```


## 运行查看效果










