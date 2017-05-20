---
title: Angular Provider
date: 2016-09-15 22:34:02
updated: 2016-09-15 22:34:02
tags:
categories: AngularJS
---

<!-- [AngularJs](http://od6sd4xau.bkt.clouddn.com/h5/AngularJs.jpg) -->

> `provider()` 是创建 `service `最底层的方式，这也是唯一一个可以使用 `.config()`方法配置创建 `service` 的方法

#### 一、创建一个可配置的服务
```javascript
	var app = angular.module('myApp', []);

	// 创建 constant 对象 "constantInput" 并传递数据
	app.constant("constantInput", {name:'Vivian',sex:'Girl'});

	// 创建可以在　config 阶段注入的服务
	app.provider('whatMyName',function(){
		var self = this;
		// 用于在 config 阶段设置值
		self.setName;

		// 这里可以注入其它依赖的服务
		self.$get = function (constantInput){
			var factory = {};  
			factory.myName = function() {
				var name='';
				if(self.setName){
					return self.setName;
				}
				return constantInput.name;
			}
			return factory;
		}
	});

	// 在 config 阶段设置默认值
	app.config(function(whatMyNameProvider){
		console.log(whatMyNameProvider.setName); //  undefined

		// 这里改变了 whatMyName 中的 setName 的值
		whatMyNameProvider.setName = 'kuku';
	});

	// 在 controller 注入使用
    app.controller('myCtrl', function($scope,whatMyName) {
		$scope.myNmae = whatMyName.myName();
	});

```
上面中需要注意，在注入到 `config` 时需要在服务名称后面 `Provider`,例如上面的 `whatMyName` 在 注入 `config` 时为 `whatMyNameProvider`。在注入其它 **服务** 或者 **控制器** 则不需要。

#### 二、在配置阶段创建服务
```javascript
	app.config(function($provide){
		$provide.provider('MathProvider', function() {
			this.$get = function() {
				var factory = {};  
				factory.multiply = function(a, b) {
					return a * b; 
				}
				return factory;
			};
		});

	});
```

#### 参考：
- [AngularJS中service，factory，provider的区别](https://my.oschina.net/tanweijie/blog/295067)
- [AngularJS Providers 详解](http://www.cnblogs.com/powertoolsteam/p/AngularJs_Providers.html)