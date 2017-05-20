---
title: Angular $q
date: 2016-09-18 22:07:58
updated: 2016-09-18 22:07:58
tags:
categories: AngularJS
feature:
---

> 本文主要是归纳总结 `AngularJs` 中的 `$q` 服务，以 [$q Api](https://code.angularjs.org/1.5.0-rc.1/docs/api/ng/service/$q) 文档为基础

### 一、使用 `constructor` 创建
可以使用类似 `ES6 Native Promise` 创建 `Promise` 
```javascript
	function asyncGreet_01(name) {
        return $q(function(resolve, reject) {
        	// 模拟耗时操作，例如读取文件，发起 Ajax 请求
            setTimeout(function() {
                if (name) {
                    resolve('Hello, ' + name + '!');
                } else {
                    reject('Greeting stranger is not allowed.');
                }
            }, 1000);
        });
    }

    var promise = asyncGreet_01('Jesse Chiu');
    promise.then(function(greeting) {
        console.log('Success: ' + greeting);
    }, function(reason) {
        console.log('Failed: ' + reason);
    });
    // Success: Hello, Jesse Chiu!
```
但是使用该方法不能像 [Native Promise](/2016/09/07/native-promise/) 那样，不能把 `throw` 转为 `reject` 来处理
```javascript
	function asyncGreet_01(name) {
        return $q(function(resolve, reject) {
        	throw 'Have some exception!!!';
        	// 模拟耗时操作，例如读取文件，发起 Ajax 请求
            setTimeout(function() {
                if (name) {
                    resolve('Hello, ' + name + '!');
                } else {
                    reject('Greeting stranger is not allowed.');
                }
            }, 1000);
        });
    }

    var promise = asyncGreet_01('Jesse Chiu');
    promise.then(function(greeting) {
        console.log('Success: ' + greeting);
    }, function(reason) {
        console.log('Failed: ' + reason);
    });
```
这是运行程序就会抛出异常。

### 二、$q Api
#### 1. The Deferred API
```javascript
	var deferred = $q.defer();
		Methods
			resolve(value)
			reject(reason)
			notify(value)
		Properties
			promise	
```
#### 2. The Promise API
```javascript
	var deferred = $q.defer();
		deferred.promise;

		Methods
			then(successCallback, errorCallback, notifyCallback)
			catch(errorCallback) – shorthand for promise.then(null, errorCallback)
			finally(callback, notifyCallback)
```
综合 1、2 两个 `API` 例子
```javascript
	function asyncGreet_01(name) {
        return $q(function(resolve, reject) {
        	// 模拟耗时操作，例如读取文件，发起 Ajax 请求
            setTimeout(function() {
                if (name) {
                    resolve('Hello, ' + name + '!');
                } else {
                    reject('Greeting stranger is not allowed.');
                }
            }, 1000);
        });
    }

	function asyncGreet_02(name) {
		var deferred = $q.defer();
		setTimeout(function() {
			deferred.notify('About to greet ' + name + ' first');
			if (name) {
				deferred.notify('About to greet ' + name + ' second');
				deferred.resolve('Hello, ' + name + '!');
			} else {
				deferred.notify('About to greet second');
				deferred.reject('Greeting stranger is not allowed.');
			}

		}, 1000);
		return deferred.promise;
	}

	var promise = asyncGreet_01('Jesse Chiu');
	promise.then(function(data){
		console.log('resolve asyncGreet_01: ' + data);
		return asyncGreet_02();
	})
	.then(function(greeting) {
		console.log('Success: ' + greeting);
	}, function(reason) {
		console.log('Failed: ' + reason);
		// throw 'Have some exception';
	}, function(update) {
		console.log('Got notification: ' + update);
		// 这里的更新消息会传递到 finally 的 notifyCallback
		return update; 
	})
	.catch(function(err){
		console.log('catch: ' + err);
	})
	.finally(function(){
		console.log('finally()');
	},function(data){
		console.log('finally notifyCallback: ' + data);
	});

	// output:
	resolve asyncGreet_01: Hello, Jesse Chiu!
	Got notification: About to greet undefined first
	Got notification: About to greet second
	Failed: Greeting stranger is not allowed.
	finally notifyCallback: About to greet undefined first
	finally notifyCallback: About to greet second
	finally()
```
上面的例子需要注意的点是在 `finally` 中的 `notifyCllback` 函数中也能得到前面的 `notify` 传出的值。


#### 3. $q.reject/resolve

$q.reject(reason)

```javascript
	var promise = asyncGreet_01();// 这里没有传人参数会使之返回 reject
	promise.then(function(data){
		console.log('resolve asyncGreet_01: ' + data);
	},function(err){
		console.log('reject-1: ' + err);
		return $q.reject('$q.reject()');
	})
	.catch(function(err){
		console.log('catch: ' + err);
	});
	// output:
	reject-1: Greeting stranger is not allowed.
	catch: $q.reject()
```
$q.resolve(value, [successCallback], [errorCallback], [progressCallback]);

```javascript
	$q.resolve(100,function(data){
		console.log('successCallback: ' + data);
	})
	.then(function(data){
		console.log('$q.resolve(100)');
	});
	// output:
	successCallback: 100
	$q.resolve(100)
```

#### 4. $q.when(value, [successCallback], [errorCallback], [progressCallback])
```javascript
	// 普通的同步函数
	function someFun(name){
        if (name) {
            return ('Hello, ' + name + '!');
        } else {
            return ('Greeting stranger is not allowed.');
        }
	}

	// 注意这里的 when 的第一个参数是个值
	$q.when(someFun('Jesse'),function(data){
		console.log('when success: ' + data);
		return data;
	},function(err){
		console.log('when fail: ' + err);
		return err;
	},function(update){
		console.log('when update: ' + update);
		return update;
	})
	.then(function(data){
		console.log('then success: ' + data);
	},function(err){
		console.log('then fail: ' + err);
	},function(update){
		console.log('then update: ' + update);
	});
	// output:
	when success: Hello, Jesse!
	then success: Hello, Jesse!
```
上面需要注意的是 `when` 的第一个参数是个值，如果改为 `$q.when(someFun)` 传的是函数名，则在 `successCallback` 得到的值则是整个函数体代码。
```javascript
	when success: function someFun(name){
	        if (name) {
	            return ('Hello, ' + name + '!');
	        } else {
	            return ('Greeting stranger is not allowed.');
	        }
		}
	then success: function someFun(name){
	        if (name) {
	            return ('Hello, ' + name + '!');
	        } else {
	            return ('Greeting stranger is not allowed.');
	        }
		}
```
#### 5. $q.all(promises)
```javascript
function asynFun_01(){
			var deferred = $q.defer();
			setTimeout(function(){
				console.log('resolve asynFun_01');
				deferred.resolve('asynFun_01');
			},500);
			return deferred.promise;
		}

	function asynFun_02(){
		var deferred = $q.defer();
		setTimeout(function(){
			console.log('resolve asynFun_02');
			deferred.resolve('asynFun_02');
		},1000);
		return deferred.promise;
	}

	function asynFun_03(){
		var deferred = $q.defer();
		setTimeout(function(){
			console.log('resolve asynFun_03');
			deferred.resolve('asynFun_03');
			// console.log('reject asynFun_03');
			// deferred.reject('reject asynFun_03');
		},200);
		return deferred.promise;
	}

	function asynFun_04(){
		var deferred = $q.defer();
		setTimeout(function(){
			console.log('resolve asynFun_04');
			deferred.resolve('asynFun_04');
		},200);
		return deferred.promise;
	}

	// 对象方式
	// $q.all({asy1:asynFun_01(),
	// 	asy2:asynFun_02(),
	// 	asy3:asynFun_03(),
	// 	asy4:asynFun_04()
	// });

	// 数组方式
	$q.all([asynFun_01(),asynFun_02(),asynFun_03(),asynFun_04()])
	.then(function(data){
		console.log('all success: ' + data);
	})
	.catch(function(err){
		console.log('catch: ' + err);
	});
	// output:
	resolve asynFun_03
	resolve asynFun_04
	resolve asynFun_01
	resolve asynFun_02
	all success: asynFun_01,asynFun_02,asynFun_03,asynFun_04
```
注意执行是并行执行，结果是按照执行顺序，如上的 `all success` 的值。
如果其中一个 reject 则执行 then 的 fallCallbck 值为 reject 的值，**但是其它的 promise 还是会继续运行**。

例如：asynFun_03 返回的是 `reject` 则结果如下
```javascript
	// output:
	reject asynFun_03
	resolve asynFun_04
	catch: reject asynFun_03 // 其中的 asynFun_03 reject 提前退出
	resolve asynFun_01
	resolve asynFun_02
```

### 三、链式编程
#### 1. 不按顺序执行
```javascript
	var asyFunArray = [asynFun_01,asynFun_02,asynFun_03,asynFun_04];
	$q.all(asyFunArray.map(function(item){
		return item();
	}))
	.then(function(data){
		console.log('resolve :' + JSON.stringify(data));
	})
	.catch(function(err){
		console.log('catch: ' + err);
	});
	// output:
	resolve asynFun_03
	resolve asynFun_04
	resolve asynFun_01
	resolve asynFun_02
	all success: asynFun_01,asynFun_02,asynFun_03,asynFun_04
```
这里方法是先把异步执行函数存到一个数组，再用数组的 <a type="text/css" href="https://msdn.microsoft.com/en-us/library/ff679976(v=vs.94).aspx">Array.map</a>  方法遍历执行并返回一个数组 promise 给 $q.all. 

#### 2. 按顺序执行
- 方法一
```javascript
	asynFun_01()
		.then(asynFun_02)
		.then(asynFun_03)
		.then(asynFun_04)
		.then(function(data){
			console.log('resolve: ' + JSON.stringify(data));
		})
		.catch(function(err){
			console.log('catch: ' + err);
		})
		.finally(function(data){
			console.log('finally: ' + data);
		});
		// output:
		resolve asynFun_01
		resolve asynFun_02
		resolve asynFun_03
		resolve asynFun_04
		resolve: "asynFun_04" 
		finally: undefined
```
该方法的缺点是不能动态编程，适合异步函数不多的情况使用

- 方法二
```javascript
	var asyFunArray = [asynFun_01,asynFun_02,asynFun_03,asynFun_04];
	asyFunArray.reduce(function(soFar,next){
		return soFar.then(function(){
			return next();
		});
	},$q.resolve())
	.then(function(data){
		console.log('resolve: ' + JSON.stringify(data));
	})
	.catch(function(err){
		console.log('catch: ' + err);
	});
	// output:
	resolve asynFun_01
	resolve asynFun_02
	resolve asynFun_03
	resolve asynFun_04
	resolve: "asynFun_04" 
```
上面使用 <a href="https://msdn.microsoft.com/library/ff679975(v=vs.94).aspx">Array.reduce</a> 方法遍历执行数组元素，并返回最后一个元素回调值。








#### 参考
- [$q Api](https://code.angularjs.org/1.5.0-rc.1/docs/api/ng/service/$q)
- [ES6 Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [Kris Kowal's Q.](https://github.com/kriskowal/q)