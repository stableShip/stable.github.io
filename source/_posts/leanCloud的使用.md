---
title: leancloud的使用
---
##leanCloud介绍
官网:[leancloud](https://leancloud.cn/)
LeanCloud 是国内的移动应用一站式云服务。
LeanCloud提供了数据存储、实时消息、统计分析以及多种扩展组件，全面涵盖移动应用开发的需求，支持 iOS、Android、Web 等多平台。
它帮助开发者摆脱后端开发负担以专注于产品创新，同时缩短开发周期、节省开发投入、快速进入市场。

----

很久之前看到baas的相关介绍,感觉自己要失业了,知道现在才有机会试用一下leancloud

----

##正文

###环境

[nodejs](http://nodejs.org/)
[leancloud命令行工具](https://leancloud.cn/docs/cloud_code_commandline.html)

####下载安装nodejs
[百度](http://www.baidu.com/s?wd=nodejs%E5%AE%89%E8%A3%85)

####下载安装leancloud命令行工具(在已安装nodejs的基础上)
`npm install -g avoscloud-code`

环境搭建完毕

<!-- more -->
###相关文档的熟悉
[nodejs项目搭建文档](https://leancloud.cn/docs/leanengine_guide-node.html)

[第三方平台接入文档](https://leancloud.cn/docs/sns.html)

坑点:[项目约束](https://leancloud.cn/docs/leanengine_guide-node.html#项目约束)
`你的项目需要遵循一定格式才会被 LeanEngine 识别并运行。
LeanEngine Node.js 项目必须有 $PROJECT_DIR/server.js 文件，该文件为整个项目的启动文件。`
**应该放在最前面的!!!!!**

在项目里面必须要有server.js文件,不要使用avoscloud new 生成的项目结构,可以参考[node-js-getting-started](https://github.com/leancloud/node-js-getting-started)项目


###安装leancloud依赖
```
npm init
npm install leanengine --save

```


###相关代码的编写
[一个基础的express项目](http://www.tuicool.com/articles/nIJfUnU)
1. 在上面项目的基础上修改app.js, 将`app.listen(3000)`修改为'module.exports = app'
2. 在app.js中添加leancloud的库的中间件`var AV = require('leanengine'); app.use(AV.Cloud)`来支持leancloud的部署检测
3. 在server.js文件中编写:
```
var AV = require('leanengine');

var APP_ID = YOUR.LC_APP_ID;
var APP_KEY = YOUR.LC_APP_KEY;
var MASTER_KEY = YOUR.LC_APP_MASTER_KEY;

AV.initialize(APP_ID, APP_KEY, MASTER_KEY);
// 如果不希望使用 masterKey 权限，可以将下面一行删除
AV.Cloud.useMasterKey();

var app = require('./app');

// 端口一定要从环境变量 `LC_APP_PORT` 中获取。
// LeanEngine 运行时会分配端口并赋值到该变量。
var PORT = parseInt(process.env.LC_APP_PORT || 3000);
var server = app.listen(PORT, function () {
  console.log('Node app is running, port:', PORT);
});

```

###本地测试
运行`avoscloud`命令
或者直接`node server.js`

###部署到leancloud
运行`avoscloud deploy`部署到测试服
设置域名:点击项目--云代码--设置--Web 主机域名 设置你想要的域名
**坑点:通过dev.+你设置的域名可以访问到测试服.**
**通过你设置的域名可以访问到正式服**

部署不成功,可以通过`node server.js`来调试你的代码

> 以上坑点都是没有先看文档造成的,所以要玩leancloud之前最好先仔细看他的文档









