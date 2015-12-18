---
title: nodejs单元测试
date: 2015-06-13 19:07:43
categories: [node]
tags: [node,mocha,blanket,rewire,should,单元测试]
---

# 相关依赖包 
* [mocha](https://github.com/mochajs/mocha) 
* [should](https://github.com/tj/should.js)
* [rewire](https://github.com/jhnns/rewire)
* [blanket](https://github.com/alex-seville/blanket)


# 第一步

## 安装依赖包 
全局安装mocha
`npm install mocha -g`

 进入项目根目录（package.json所在路径）
`npm install --dev`


## 创建一个单元测试

进入单元测试文件目录：
`cd test`

添加文件，并填写单元测试内容
[如何编写单元测试](http://mochajs.org/)

查看是否通过测试
`mocha ./`

<!-- more -->
##查看单元测试代码覆盖率

进入项目根目录（Makefile，package.json所在目录）

运行项目中所有单元测试用例
`make test`

运行代码覆盖率检测，生成检测报告（可以不用第一步）
`make test-cov`  

双击打开coverage.html查看代码覆盖率



# 配置文件汇总说明
* package.json
``` josn
  "config": {
    "blanket": {
      "pattern": "///[\\w-]+\\.js$/",
      "data-cover-never": [
        "node_modules",
        "public"
      ],
      "data-cover-reporter-options": {
        "shortnames": true
      }
    }
  }
```

指定了代码覆盖率检测库：blanket相关配置


*Makefile

``` json
TESTS = $(shell find  test -type f -name "*.test.js")
REPORTER = spec
TIMEOUT = 80000
MOCHA_OPTS =

test: 
	@NODE_ENV=test /usr/local/bin/mocha \
		--reporter $(REPORTER) \
		--timeout $(TIMEOUT) \
		$(MOCHA_OPTS) \
		$(TESTS)

test-cov:
	@rm -f coverage.html
	@$(MAKE) test MOCHA_OPTS='--require blanket' REPORTER=html-cov > coverage.html
	@ls -lh coverage.html

test-all: test test-cov

.PHONY: test-cov test test-all

```

`TESTS`指定查找当前目录下test下的所有单元测试文件

`test` 指定了`make test`运行的相关操作：执行mocha单元测试

`test-cov`指定了`make test-cov`运行的相关操作：执行mocha单元测试，并生成代码覆盖率报告coverage.html

