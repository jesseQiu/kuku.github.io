---
title: Caller 和 Callee 区别
date: 2017-03-14 18:49:25
tags:
categories: JavaScript
---

## 一、Function.caller
返回 **调用** 指定函数的 **函数**
如果一个函数 `f` 是在全局作用域内被调用的,则 `f.caller` 为 `null` ,相反,如果一个函数是在另外一个函数作用域内被调用的,则 `f.caller` 指向调用它的那个函数。
该属性的常用形式 `arguments.callee.caller` 替代了被废弃的 [arguments.caller](https://developer.mozilla.org/zh-cn/JavaScript/Reference/Functions_and_function_scope/arguments/caller "zh-cn/JavaScript/Reference/Functions_and_function_scope/arguments/caller");
```js

function myFunc() {
   var _caller = myFunc.caller;

   console.log('myFunc(): ' + typeof _caller);
   if (_caller === null) {
      console.log("该函数在全局作用域内被调用!");
   } else{
      console.log("调用我的是函数是: " + _caller.toString());
   }
}

function testFun(){
	myFunc();
}

myFunc();// 该函数在全局作用域内被调用!
testFun(); // 调用我的是函数是: function testFun(){myFunc();}
```

## 二、arguments.callee
`arguments.callee` 属性包含当前正在执行的函数。
`callee` 是 `arguments` 对象的一个属性。它可以用于引用该函数的函数体内当前正在执行的函数。这在函数的名称是未知时很有用，例如在没有名称的函数表达式 (也称为“匿名函数”)内。

- arguments.length: 实参个数
- arguments.callee.length: 形参个数

```js
function myFunc(a,b,c) {
   var _callee = arguments.callee;
   console.log('myFunc(): _callee 类型：' + typeof _callee);
   console.log('函数体： _callee: ' +_callee.toString()); 
   console.log('实参个数: arguments.length: ' +　arguments.length);
   console.log('形参个数: _callee.length: ' +_callee.length);
}
myFunc();
```
应用实例:

1. 精准定时器:
```js
// 下面代码在匿名函数中调用自身，来确保定时器准确的以每隔 2S 调用一次
var i = 1;
var timer = setTimeout(function() {
  alert(i++);
  timer = setTimeout(arguments.callee, 2000);
}, 2000);
```
2. 事件取消：
```js
// 绑定事件
document.addEventListener('click',function(){
	console.log('add click event!!');
	// 点击一次后解绑自己
	document.removeEventListener('click',arguments.callee,false);
},false);
```

> 警告：在严格模式下，第 5 版 ECMAScript (ES5) 禁止使用 `arguments.callee()`。当一个函数必须调用自身的时候, 避免使用 `arguments.callee()`, 通过要么给函数表达式一个名字，，要么使用一个函数声明.

```js
function f1(){
  "use strict";
  f1.caller; 
}
f1();// 报错: VM848:3 Uncaught TypeError: 'caller' and 'arguments' are restricted function properties and cannot be accessed in this context.(…)
```



## 参考
[Function.caller](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/caller)
[arguments.callee](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/arguments/callee)