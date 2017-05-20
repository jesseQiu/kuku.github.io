---
title: Angular copy、extend、merge
date: 2016-09-16 10:55:01
updated: 2016-09-16 10:55:01
tags:
categories: AngularJS
feature:
---

> 在开发中经常需要对数组或者对象进行拷贝，这时就会用到 `angular.copy/extend/merge` 这些 `API` 但是它们之间又有差别，本文主要是总结它们之间的用法和差别。在开始之前有必要先了解下， **浅拷贝** 和 **深拷贝** 的差别。

#### 一、JS 的拷贝
1. 浅拷贝
	JavaScript 存储对象都是存地址的，所以浅拷贝就是复制一份引用，引用对象都指向同一份数据，并且都可以修改这份数据。
	```javascript
		var array1 = [1,3,'jesse'],
			obj1 = {name:'jesse',sex:'man'},
			array2,obj2;

			// 浅拷贝
			array2 = array1;
			obj2 = obj1;
			
			console.log(array2); // [1, 3, "jesse"]
			console.log(obj2); // Object {name: "jesse", sex: "man"}

			// 改变原先对象值
			array1[2] = 'vivian';
			obj1.name = 'vivian';

			// 拷贝的值也一起改变
			console.log(array2);  // [1, 3, "vivian"]
			console.log(obj2); // Object {name: "vivian", sex: "man"}
	```

2. 深拷贝
	深拷贝则是复制变量值，对于非基本类型的变量，则递归至基本类型变量后，再复制。
	```javascript
		var cloneObj = function(obj){
		    var str, newobj = obj.constructor === Array ? [] : {};
		    if(typeof obj !== 'object'){
		        return;
		    } else if(window.JSON){
		        str = JSON.stringify(obj), //序列化对象
		        newobj = JSON.parse(str); //还原
		    } else {
		        for(var i in obj){
		            newobj[i] = typeof obj[i] === 'object' ? cloneObj(obj[i]) : obj[i]; 
		        }
		    }
		    return newobj;
		};

		var array1 = [1,2,'jesse',{name:'vivian',sex:'girl'}],
			obj1 = {name:'jesse',sex:'man',addr:[3,4,{name:'kuku',sex:'man'}]},
			array2=[],obj2={};

			array2 = cloneObj(array1);
			obj2 = cloneObj(obj1);

			console.log(array2); // [1, 2, "jesse", Object]
			console.log(obj2); // Object {name: "jesse", sex: "man", addr: Array[3]}
			console.log(array1.obj === array2.obj); // false
			console.log(obj1.obj === obj2.obj); // false

			// 改变 boj1 和 array1 中的值
			array1[0]='beauty';
			obj1.name = 'vivian';

			console.log(array2); // [1, 2, "jesse", Object]
			console.log(obj2); // Object {name: "jesse", sex: "man", addr: Array[3]}
	```
	上面的深拷贝，改变了 `array1` 和 `obj1` 中的值后，`array2` 和 `obj2` 的值并未改变。

#### 二、angular.copy()

`angular.copy(source,destination)` 属于 **深度拷贝**

```javascript
	var mySource = {'name' : 'sakshi', 'age' : '24', 'obj' :{'key':'value'}}
	var myDest = {}

	// 深度拷贝
	angular.copy(mySource, myDest);
	console.log(myDest); // {'name' : 'sakshi', 'age' : '24', 'obj' :{'key':'value'}}
	console.log(mySource.obj === myDest.obj); // false

	// 改变原始值
	mySource.name = 'jesse';
	// myDest 的值没有变
	console.log(myDest); // {'name' : 'sakshi', 'age' : '24', 'obj' :{'key':'value'}}
	console.log(mySource); // {'name' : 'jesse', 'age' : '24', 'obj' :{'key':'value'}}
```
上面的 `myDest = {name:'vivian',obj:{key:'apple',size:100}}` 也就是不为空对象，运行后的结果还是一样。可以看出 `angular.copy(source,destination)` 是把 `source` 的值完整拷贝一份到 `destination`。

#### 三、angular.extend()

`angular.extend(destination, src1, src2 …)` 是属于 **浅拷贝** (这里的浅拷贝是指 `src` 中的属性包含的 **对象**，如果属性不是对象则是复制拷贝)，也就是 `extend` 没有递归拷贝对象。

```javascript
	var mySource1 = {'name' : 'neha', 'age' : '26', obj2 : {sex:'man',addr:['china',{city:'fuzhou'}]}};
	var mySource2 = {'course' : 'MCA',obj3:{year:2016,month:9}};
	var myDest = {};

	// 拷贝
	angular.extend(myDest, mySource1,mySource2)
	console.log(myDest); // Object Object {name: "neha", age: "26", obj2: Object, course: "MCA", obj3: Object}
	console.log(myDest.obj2.addr[0]); // 'china'
	console.log(myDest.obj2.addr[1].city); // 'fuzhou'

	console.log(myDest.obj === mySource1.obj); // true
	console.log(myDest.obj === mySource2.obj); // true

	// 改变 mySource1 值
	mySource1.name = 'jesse'; // 改变属性值
	mySource1.obj2.sex = 'girl'; // 改变属性 -> 对象 -> 属性的值

	// 改变 mySource2 值
	mySource2.course = 'front-end'; // 改变属性值
	mySource2.obj3.year = 2018; // 改变属性 -> 对象 -> 属性的值

	console.log(myDest); // Object {name: "neha", age: "26", obj2: Object, course: "MCA", obj3: Object}
	console.log(myDest.obj2.sex); // 'girl'
	console.log(myDest.obj3.year); // 2018
	
	console.log(mySource1); // Object {name: "jesse", age: "26", obj2: Object}
	console.log(mySource2); // Object {course: "front-end", obj3: Object}

```
上面的代码可以看出，在 `extend()` 后，改变 `mySource1.name` 和 `mySource2.course` 的值后，`myDest` 中的值并没改变。但是改变其中属性值，`mySource1.obj2.sex = 'girl'` 和 `mySource2.obj3.year = 2018;` 后 `myDest` 的值也一起变了。由此可以看出 `extend` 只是拷贝了第一层的值，当属性值为对象时则是引用。

如果 myDest 和 mySource1 中有相同名称的属性值情况如何：
```javascript
	var mySource = {'name' : 'neha', 'age' : '26', obj2 : {sex:'man',addr:['china',{city:'fuzhou'}]}};
	var myDest = {'name':'vivian',obj2:{sex:'girl'}};

	angular.extend(myDest,mySource);
	console.log(myDest); // Object {name: "neha", obj2: Object, age: "26"}
	console.log(myDest.name); // 'neha'
	console.log(myDest.obj2.sex); // 'man'
```
由上面的例子可以看出，使用 `extend` 会覆盖相同属性名的值。


#### 四、angular.merge()

由于 `angular.extend()` 只能拷贝第一级的属性值，如果要递归拷贝属性中的对象，则需要用 `angular.merge()`

```javascript
	var mySource1 = {'name' : 'neha', 'age' : '26', obj2 : {sex:'man',addr:['china',{city:'fuzhou'}]}};
	var mySource2 = {'course' : 'MCA',obj3:{year:2016,month:9}};
	var myDest = {};

	// 拷贝
	angular.merge(myDest, mySource1,mySource2);
	console.log(myDest); // Object Object {name: "neha", age: "26", obj2: Object, course: "MCA", obj3: Object}
	console.log(myDest.obj2.addr[0]); // 'china'
	console.log(myDest.obj2.addr[1].city); // 'fuzhou'

	console.log(myDest.obj === mySource1.obj); // true
	console.log(myDest.obj === mySource2.obj); // true

	// 改变 mySource1 值
	mySource1.name = 'jesse'; // 改变属性值
	mySource1.obj2.sex = 'girl'; // 改变属性 -> 对象 -> 属性的值

	// 改变 mySource2 值
	mySource2.course = 'front-end'; // 改变属性值
	mySource2.obj3.year = 2018; // 改变属性 -> 对象 -> 属性的值

	console.log(myDest); // Object {name: "neha", age: "26", obj2: Object, course: "MCA", obj3: Object}
	console.log(myDest.obj2.sex); // 'man'
	console.log(myDest.obj3.year); // 2016
	
	console.log(mySource1); // Object {name: "jesse", age: "26", obj2: Object}
	console.log(mySource2); // Object {course: "front-end", obj3: Object}
```
上面代码可以看出，改变 `mySource1` 和 `mySource1` 中的属性对象的值，`myDest` 的值并没改变。


#### 参考
- [js 深浅拷贝差别](http://www.zhihu.com/question/23031215)
- [AngularJS : copy vs extend](http://www.tothenew.com/blog/angularjs-copy-vs-extend/)
- [angular.extend](http://blog.csdn.net/itsonglin/article/details/47428955)
