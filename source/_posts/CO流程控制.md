---
title: co流程控制
date: 2016-04-10 16:07:43
tags: [co,nodejs]
categories: [co,nodejs]
---

# co流程控制

## nodejs常见异步流程

#### 使用回调

``` javascript

fs.readFile("./file.js",function(err,data){
    console.log(data.toString());
})

```

#### 使用promise

``` javascript

var Promise = require("bluebird")
var fs = require("fs");
fs = Promise.promisifyAll(fs);
fs.readFileAsync("./test.js").then(function(data){
    console.log(data.toString());
})

```

#### 使用co+promise

``` javascript

var fs = require("fs");
var Promise = require("bluebird")
fs = Promise.promisifyAll(fs);
co(function* (){
    var data = yield fs.readFileAsync('./test.js');
    console.log(data.toString());
})

```



结论：使用co模块编写代码，形式与同步代码一致，没有回调和then，逻辑更加清晰。

<!-- more -->

## co 并发

`并行执行多个函数，每个函数都是立即执行，不需要等待其它函数先执行。传给最终callback的数组中的数据按照tasks中声明的顺序，而不是执行完成的顺序。 `

``` javascript

var parallel = require('co-parallel');
var Promise = require("bluebird")
var co = require("co");
var fs = require("fs");

co(function* () {
	// 创建多个promise
    var promise = new Promise(function(resolve){
        setTimeout(function(){
            resolve(1);
        },200)
    })
    var promises = [];
    // 将所有promise保存到一个数组
    for(var i=0;i<100000;i++){
        promises.push(promise);
    }
    var start = new Date().getTime();
    // yield 这个数组
    // var res = yield promises
    // bluebird 的promise自2.0之后不允许yield 数组（Promise.all也无法yield），相关链接https://github.com/petkaantonov/bluebird/pull/237 
    // 所以必须使用co-parallel 进行包装，并且能够控制并发数量，默认并发数：5
    var res = yield parallel(promises)
    console.log(res,res.legth,new Date().getTime()- start);
});


```


## co 串联

`有多个异步函数需要依次调用，一个完成之后才能执行下一个。各函数之间没有数据的交换，仅仅需要保证其执行顺序。这时可使用series`

``` javascript

// promise：
Promise.mapSeries([{timeout: 100, value: 1},
        {timeout: 200, value: 2}], function (item) {
        return Promise.delay(item.timeout, item.value);
}).then(function(results){
		console.log(results) // [ 1, 2 ]
})



// co:

co(function* (){
     var results = yield Promise.mapSeries([{timeout: 100, value: 1},
        {timeout: 5000, value: 2}], co.wrap(function* (item) {
        return Promise.delay(item.timeout, item.value);
    }))
    console.log(results);
}).catch((err)=>{
    console.log(err);
})


```

`mapSeries 和 each 的区别： mapSeries返回处理函数处理后的结果，each返回传入参数`
`mapSeries 和 map 的区别： mapSeries等待上一个处理函数执行完毕再执行下一个，map直接执行`
[参考链接](http://bluebirdjs.com/docs/api/promise.mapseries.html)




## co错误处理

``` javascript

co(function* (){
    try{
     // 针对某个特殊的错误进行try，catch
        try {
            throw "test"
        }catch (err){
            console.log(err); //test
        }
        //可以继续执行
        var results = yield Promise.mapSeries([{timeout: 500, value: 1},
            {timeout: 5000, value: 2},{timeout: 100, value: 3}], co.wrap(function* (item) {
            console.log(item)
            return Promise.delay(item.timeout, item.value)
        }))
        console.log(results);
    }catch(err){
        console.log(err);
    }

})

```



## co编写函数与调用

``` javascript

// 函数
function test() {
    return co(function* () {
        try {
            var results = yield Promise.resolve([1,2])
            return results;
        } catch (err) {
            console.log(err);
        }
    })
}

// 调用
co(function* (){
    var results = yield test();
    console.log(results); //[1,2]
})

```



