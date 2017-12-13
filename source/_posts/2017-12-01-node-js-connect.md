---
title: Connect 
date: 2017-12-01 14:34:39
updated: 2017-12-01 14:34:39
categories: Node.js
---

> [connect](https://github.com/senchalabs/connect/) 和 [express](https://github.com/visionmedia/express)出自同一作者 [TJ Holowaychuk](https://npmjs.org/~tjholowaychuk)（之后简称TJ）。从依赖性看，express 基于 connect；但从 2 个项目的 git 提交历史来看，实际上先有 express 项目（2009-6-27），2010-5-27 前后 connect 从 express 项目分化出来（express 0.12.0）。express 0.12.0 这个版本实际上已经有了中间件（middleware）雏形，只不过当时还叫 plugin，那时已经有了logger，static，session等中间件。  

## Connect

> Connect is a middleware framework for node.

[Connect](http://www.senchalabs.org/connect/)是一个 node 中间件（middleware）框架。如果把一个 http 处理过程比作是污水处理，中间件就像是一层层的过滤网。每个中间件在 http 处理过程中通过改写 request 或（和）response 的数据、状态，实现了特定的功能。这些功能非常广泛，下图列出了 connect 所有
`内置` `中间件`和部分`第三方中间件`。 这里能看到[完整的中间件列表](https://github.com/senchalabs/connect/wiki)。  
下图根据中间件在整个http处理流程的位置，将中间件大致分为 

1.  **Pre-Request** 通常用来改写 request 的原始数据
2.  **Request/Response** 大部分中间件都在这里，功能各异
3.  **Post-Response** 全局异常处理，改写 response 数据等

![](http://files.cnblogs.com/luics/connect-middleware.zip)

### 为何要有 Connect？

下面的代码展示了使用 connect 和 node 原生 api 写一个静态文件服务器：
使用 connect 实现的静态文件处理
```js
var connect = require('connect');
connect(connect.static(__dirname + '/public')).listen(//监听
    3000,
    function() {
        console.log('Connect started on port 3000');
    }
);
```
使用 node 原生 api 实现
```js
var http = require('http');
http.createServer(
    function(req, res) {
        var url = require('url');
        var fs = require('fs');
        var pathname = __dirname + '/public' + url.parse(req.url).pathname;
        //读取本地文件
        fs.readFile(
            pathname,
            function(err, data) {
                //异常处理
                if (err) {
                    res.writeHead(500);
                    res.end('500');
                }
                else {
                    res.end(data);
                }
            }
        );
    }
).listen(//监听
    3001,
    function() {
        console.log('http.Server started on port 3001');
    }
);
```
尽管 node 原生 api 已经花费这么些行代码，但其实仍然留下一个简单静态文件服务器的诸多方面未经处理，比如：404 等异常未处理、没有基本的文件路径安全验证（实际上我们可以访问到整个 os 文件系统）、全局异常处理等等；与此同时 connect 已经将这些问题都处理好了。


## 

## 参考:
- [connect & express 介绍](http://www.cnblogs.com/luics/archive/2012/11/28/2775206.html)
- [connect 中间件](http://blog.fens.me/nodejs-connect/)