---
title: Native Promise
date: 2016-09-07 18:49:25
tags: Promise
categories: JavaScript
---

> 本文主要是记录原生 Promise 使用方法，以及在项目开发中遇到的问题点。如果对 Promise 还不了解的推荐阅读 [JavaScript Promise 迷你书](http://liubin.org/promises-book/)

### 一、Promise 基础用法

#### 1.创建一个 Promise 函数
```javascript
function asyncFunction() {
  return new Promise(function(resolve, reject) {
      setTimeout(function() {
          resolve('Async Hello world');
      }, 16);
  });
}

asyncFunction()
.then(function(value) {
    console.log(value); // => 'Async Hello world'
})
.catch(function(error) {
    console.log(error);
});
```
*`.catch` 只是 `promise.then(undefined, onRejected)` 的别名而已， 如下代码也可以完成同样的功能*

#### 2.Promise只能进行异步操作
```javascript
var promise = new Promise(function (resolve){
	console.log("inner promise"); // 1
	resolve(42);
});
promise.then(function(value){
    console.log(value); // 3
});
console.log("outer promise"); // 2

运行结果:
inner promise // 1
outer promise // 2
42            // 3
```

#### 3.promise chain

- 使用 reject 
```javascript
function taskA() {
  	console.log("Task A");
}
function taskB() {
    console.log("Task B");
}
function onRejected(error) {
    console.log("Catch Error: A or B", error);
}
function finalTask() {
    console.log("Final Task");
}

var promise = Promise.resolve();
promise
.then(taskA)
.then(taskB)
.catch(onRejected)
.then(finalTask);
```
- 使用 throw 抛出异常
```javascript
function taskA() {
  console.log("Task A");
  	throw new Error("throw Error @ Task A")
}
function taskB() {
    console.log("Task B");// 不会被调用
}
function onRejected(error) {
    console.log(error);// => "throw Error @ Task A"
}
function finalTask() {
    console.log("Final Task");
}

var promise = Promise.resolve();
promise
    .then(taskA)
    .then(taskB)
    .catch(onRejected)
    .then(finalTask);
```

#### 4.promise chain 中如何传递参数  

```javascript
function doubleUp(value) {
  	return value * 2;
}
function increment(value) {
    return value + 1;
}
function output(value) {
    console.log(value);// => (1 + 1) * 2
}

var promise = Promise.resolve(1);
promise
.then(increment)
.then(doubleUp)
.then(output)
.catch(function(error){
    // promise chain中出现异常的时候会被调用
    console.error(error);
});
```

#### 5.then 写法  
- 错误写法  
```javascript
function badAsyncCall() {
    var promise = Promise.resolve();
    promise.then(function() {
        // 任意处理
        return newVar;
    });
    return promise;
}
```
*这种写法有很多问题，首先在 `promise.then` 中产生的异常不会被外部捕获，此外，也不能得到 `then` 的返回值，即使其有返回值。由于每次 `promise.then` 调用都会返回一个新创建的 `promise` 对象*

- 正确写法  
```javascript
function anAsyncCall() {
    var promise = Promise.resolve();
    return promise.then(function() {
        // 任意处理
        return newVar;
    });
}
```

#### 6.Promise.all
```javascript
// `delay`毫秒后执行resolve
function timerPromisefy(delay) {
    return new Promise(function (resolve) {
        setTimeout(function () {
            resolve(delay);
        }, delay);
    });
}
var startDate = Date.now();
// 所有promise变为resolve后程序退出
Promise.all([
    timerPromisefy(1),
    timerPromisefy(32),
    timerPromisefy(64),
    timerPromisefy(128)
]).then(function (values) {
    console.log(Date.now() - startDate + 'ms');
    // 約128ms
    console.log(values);    // [1,32,64,128]
});
```
*`Promise.all` 的 `Promise` 不是按顺序执行，当时值是按顺序传递*

#### 7.Promise.race
```javascript
var winnerPromise = new Promise(function (resolve) {
      setTimeout(function () {
          console.log('this is winner');
          resolve('this is winner');
      }, 4);
  });
var loserPromise = new Promise(function (resolve) {
        setTimeout(function () {
            console.log('this is loser');
            resolve('this is loser');
        }, 1000);
    });
// 第一个promise变为 resolve 后程序停止
Promise.race([winnerPromise, loserPromise]).then(function (value) {
    console.log(value);    // => 'this is winner'
});
```

#### 8.Promise.resolve(value)/reject(value)
- Promise.resolve(value)
```javascript
Promise.resolve(42);

// 可以认为是下面代码的语法糖
new Promise(function(resolve){
  	resolve(42);
});

// 具体可以如下使用
Promise.resolve(42).then(function(value){
  	console.log(value);
});

```
- Promise.reject(value)
```javascript
Promise.reject(new Error("出错了"));

// 可以认为是下面代码的语法糖
new Promise(function(resolve,reject){
    reject(new Error("出错了"));
});

// 具体可以如下使用
Promise.reject(new Error("BOOM!")).catch(function(error){
  	console.error(error);
});

```

#### 9.then or catch?
```javascript
function throwError(value) {
  	// 抛出异常
  	throw new Error(value);
}
// <1> onRejected不会被调用
function badMain(onRejected) {
    return Promise.resolve(42).then(throwError, onRejected);
}
// <2> 有异常发生时onRejected会被调用
function goodMain(onRejected) {
    return Promise.resolve(42).then(throwError).catch(onRejected);
}
// 运行示例
badMain(function(){
    console.log("BAD");
});
goodMain(function(){
    console.log("GOOD");
});
```
*上在上面的代码中， badMain 是一个不太好的实现方式（但也不是说它有多坏）， goodMain 则是一个能非常好的进行错误处理的版本。
为什么说 badMain 不好呢？，因为虽然我们在 .then 的第二个参数中指定了用来错误处理的函数，但实际上它却不能捕获第一个参数 onFulfilled 指定的函数（本例为 throwError ）里面出现的错误*



### 二、传值问题 

由于 Promise.resolve/reject 只能传一个参数,如果需要传递多个值可以参考下面的方法

1. 通过 {} 或者 []

```javascript
function fun01(){
	return new Promise(resolve,reject){
		setTimeout(function(){
			resolve({name:'liu',age:'20'});
		},100);
	}
}
fun01()
.then(function(data){
	console.log(data);
	},function(err){
	console.log(err);
});
```

2. 通过调用 `apply` 方法传递参数

```javascript
if (!Promise.prototype.spread) {
      Promise.prototype.spread = function (successFn,failFn) {
          return this.then(function(args){
          	return successFn.apply(this, args);
          },function(args){
			return failFn.apply(this,args);
          })
      };
  }

function fun02(){
	return new Promise(resolve,reject){
		setTimeout(function(){
			resolve(['liu',20]);
		},100);
	}
}

fun02()
.spread(function(name,age){
	// TODO ...
})
.then(success,fail);
```


### 三、简化 Promise 函数封装

- 优化前函数
```javascript
function fun01(){
	return new Promise(resolve,reject){
		setTimeout(function(){
			resolve('Hello');
		},100);
	}
}

function fun02() {
	return Promise(resolve, reject) {
		if ( /*condition*/ ) {
			resolve(data);
		} else {
			fun01()
				.then(function() {
					// ...
					resolve(data);
				}, function(err) {
					reject(err);
				});
		}
	}
}

fun02()
	.then(success,fail);
```

	上面的 `fun02` 可以简化为

- 优化后的函数
```javascript
	function fun02() {

		if ( /*condition*/ ) {
			Promise.resolve();
		} else {
			return fun01()
				.then(function() {
					// ...
					return data; // 这里使用 return 可以把值传递给 success 函数。
				});
		}
	}
```
	这里的 `fun01` 如果返回的是 `reject`, 一样可以透传到 `fail` 函数。 

### 四、递归函数封装
```javascript
function promiseFn(i){
	return promise = new Promise(function(resolve,reject){
		console.log('promiseFn(): ' + i);
		(function _promiseFn(i){
			console.log('_promiseFn(): ' + i);
			setTimeout(function(i){
				console.log('tiemout: ' + i);
				if(i < 5){
					console.log('invoke: ' + i);
					return _promiseFn(++i);
				}else{
					resolve('promiseFn resolve');
				}
			},1000,i);
		})(i);
	});
}
```
*通过内部提取封装调用一个立即执行函数方法*

### 五、思考
#### 1. 打印的 log 顺序是?
```js
console.log(`one`);
let doSth = new Promise((resolve, reject) => {
  console.log('hello');
  resolve();
});
console.log(`two`);
setTimeout(() => {
  doSth.then(() => {
    console.log('over');
  })
}, 10000);
console.log(`three`);
```

### 2. 打印的 log 顺序是?
```js
setTimeout(function() {
  console.log(1)
}, 0);

new Promise(function executor(resolve) {
  console.log(2);
  for( var i=0 ; i<10000 ; i++ ) {
    i == 9999 && resolve();
  }
  console.log(3);
}).then(function() {
  console.log(4);
});
console.log(5);
```

### 参考
- 如果要在不支持原生 `Promise` 使用，可以到官方推荐的网站下载 [es6-promise](https://github.com/stefanpenner/es6-promise) 库。