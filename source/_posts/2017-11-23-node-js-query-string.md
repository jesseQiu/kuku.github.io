---
title: Query String 接口
date: 2017-11-22 14:34:39
updated: 2017-11-22 14:34:39
categories: Node.js
---

## Query String
从名字就可以看出是一个和参数相关的帮助类,node.js 原生自带,直接 require('querystring') 即可使用.
此类一共包括 4 个方法:
- querystring.stringify(obj, [sep], [eq]) 
- querystring.parse(str, [sep], [eq], [options])
- querystring.escape    
- querystring.unescape  
[内参数]表示可选参数, [sep]指分隔符 默认& , [eq]指分配符 默认=

## 一、querystring.stringify(obj,[sep],[eq])

对象格式化成参数字符串 ,obj 就是要格式化的对象,必选参数.
```js
var param = require('querystring');
var obj={name:"一介布衣",url:"http://yijiebuyi.com"};
//没有指定分隔符和分配符,并且自动编码汉字
var param= querystring.stringify(obj);
console.log(param);

//指定了分隔符和分配符
param=querystring.stringify(obj,'|','*');
console.log(param);
```

## 二、querystring.parse(str, [sep], [eq], [options]) 
参数字符串格式化成对象
```js
var obj={name:"一介布衣",url:"http://yijiebuyi.com"};
// 我们把 param 字符串格式化成对象,使用默认分隔分配符
var param= querystring.stringify(obj);
var newobj=querystring.parse(param);
//打印出来格式化后的数据类型 和 内容.
console.log(typeof newobj,newobj);
```

可以看到格式化以后是 object 类型,并且汉字自动解码显示出来.
```js
//当覆盖分割和分配符,如下:
param=querystring.stringify(obj,'|','*');
console.log(param);

//然后解析:
param=querystring.stringify(obj,'|','*');
console.log(param);
```

## 三、querystring.escape
参数编码
```js
var param="一介布衣&" 
console.log(querystring.escape(param));
```

## 四、querystring.unescape  

参数解码
```js
var param="一介布衣&http://yijiebuyi.com";
console.log(querystring.escape(param));
console.log(querystring.unescape(querystring.escape(param)));
```
直接对上面编码后的参数字符串进行解码

## 参考:
- [QueryString](https://nodejs.org/dist/latest-v8.x/docs/api/querystring.html)