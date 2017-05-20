---
title: 搭建 Node.js Https 服务器
date: 2017-02-13 23:34:39
updated: 2017-02-13 23:34:39
tags:
categories: Node.js
feature:
---

## 一、运行环境
1. 操作系统：windows 8.1
2. 安装 [nodejs](https://nodejs.org/en/)
3. 安装 [msysgit](https://git-for-windows.github.io/)
4. 安装 express 环境：
	```bash
	express -e  nodejs-https
	cd nodejs-https && npm install
	```

## 二、使用 openssl 生成证书
由于 [msysgit](https://git-for-windows.github.io/) 工具自带了 **openssl** 工具，所有可以直接在 `Git Bash` 中直接生成
1. 生成私钥 key:
	```bash
	openssl genrsa -out privatekey.pem 1024
	```
2. 通过私钥生成 CSR 证书签名
	```bash
	openssl req -new -key privatekey.pem -out certrequest.csr
	```
	如果是 windows 系统，这步会报 `Unable to load config info from /usr/local/ssl/openssl.cnf` 错误，这是因为在 windows 系统中并没有 `/usr/local/ssl/openssl.cnf` 这个目录。所以解决思路就是
	- 找到 openssl 中的安装目录: 例如: `C:\Program Files (x86)\Git\ssl\openssl.cnf`
	- 在 **系统环境变量**中增加 `OPENSSL_CNF` 键，值就是上面找到 `C:\Program Files (x86)\Git\ssl\openssl.cnf`
	**注意：上面的设置环境变量的值，必须必须要完整的路径**

3. 通过私钥和证书签名生成证书文件
	```bash
	openssl x509 -req -in certrequest.csr -signkey privatekey.pem -out certificate.pem
	```
	这时就生成了:
	- privatekey.pem: 私钥
	- certrequest.csr: CSR证书签名
	- certificate.pem: 证书文件

## 二、修改启动的 app.js 文件
```js
//最下面
var https = require('https')
    ,fs = require("fs");

var options = {
    key: fs.readFileSync('./privatekey.pem'),
    cert: fs.readFileSync('./certificate.pem')
};

https.createServer(options, app).listen(3011, function () {
    console.log('Https server listening on port ' + 3011);
});
```
启动服务器：
```js
$>node app.js
Express server listening on port 3000
Https server listening on port 3011
```

## 参考:
- [Nodejs 创建 HTTPS 服务器](http://blog.fens.me/nodejs-https-server/)
- [Nodejs+Express 创建 HTTPS 服务器](http://www.jianshu.com/p/853099ae2edd)
