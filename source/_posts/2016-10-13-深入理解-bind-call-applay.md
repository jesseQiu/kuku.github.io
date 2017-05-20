---
title: 深入理解 bind、call、apply
date: 2016-10-13 11:23:14
updated: 2016-10-13 11:23:14
tags:
categories: JavaScript
feature:
---

## 一、apply、call 

### 1. 初识

在 javascript 中，call 和 apply 都是为了改变某个函数运行时的上下文（context）而存在的，换句话说，就是为了改变函数体内部 `this` 的指向。

javaScript 的一大特点是，函数存在 **定义时上下文** 和 **运行时上下文** 以及 **上下文是可以改变的** 这样的概念。

先看个例子：

```javascript
function fruits() {}

fruits.prototype = {
	color: 'red',
	say: function() {
		console.log('My color is' + this.color);
	}
}

var apple = new fruits;
apple.say();    // My color is red
```

但是如果我们有一个对象 `banana= {color : 'yellow'}` ,我们不想重新定义 say 方法，那么我们可以通过 call 或 apply 用 apple 的 say 方法：

```javascript
banana = {
	color: 'yellow'
}
apple.say.call(banana);     //My color is yellow
apple.say.apply(banana);    //My color is yellow
// 如果传人的是 null 或者 空值：
apple.say.call();     //My color is undefined
apple.say.apply(null);    //My color is undefined
```
所以，可以看出 call 和 apply 是为了动态改变 this 而出现的，当一个 object 没有某个方法（上面例子中 banana 没有 say 方法），但是其他的有（例如: apple 有 say 方法），我们可以借助 call 或 apply 用其它对象的方法来操作。

### 2. apply、call 的区别

对于 apply、call 二者而言，作用完全一样，只是接受 **参数** 的方式不太一样。例如，有一个函数定义如下：

```javascript
var func = function(arg1, arg2) {
     
};
就可以通过如下方式来调用：
func.call(this, arg1, arg2);
func.apply(this, [arg1, arg2]);
```

其中 this 是你想指定的上下文，他可以是任何一个 JavaScript 对象(JavaScript 中一切皆对象)，call 需要把参数按 **顺序** 传递进去，而 apply 则是把参数放在 **数组** 里。

JavaScript 中，某个函数的参数数量是不固定的，因此要说适用条件的话，当你的参数是明确知道数量时用 call 。

而不确定的时候用 apply，然后把参数 push 进数组传递进去。当参数数量不确定时，函数内部也可以通过 arguments 这个数组来遍历所有的参数。

为了巩固加深记忆，下面列举一些常用用法：

#### 2.1 数组之间追加
```javascript
var array1 = [12,'foo',{name:'Joe'},-2458]; 
var array2 = ['Doe' , 555 , 100]; 
Array.prototype.push.apply(array1, array2); // 这里用 apply 第二个参数是一个数组

// 等价于:
array1.push('Doe' , 555 , 100);

// ES6 方法:
array1.push('Doe', ...array2);
/* array1 值为  [12 , 'foo' , {name 'Joe'} , -2458 , 'Doe' , 555 , 100] */
```
#### 2.2 获取数组中的最大值和最小值
```javascript
var numbers = [5, 458 , 120 , -215 ]; 
var maxInNumbers = Math.max.apply(Math, numbers),   //458
// 等价于: 
var maxInNumbers = Math.max.call(Math,5, 458 , 120 , -215); //458
// number 本身没有 max 方法，但是 Math 有，我们就可以借助 call 或者 apply 使用其方法。
```
#### 2.3 验证是否是数组(前提是 toString() 方法没有被重写过)
```javascript
function isArray(obj){ 
  return Object.prototype.toString.call(obj) === '[object Array]' ;
}
```
#### 2.4 类（伪）数组使用数组方法
```javascript
var divElements = document.getElementsByTagName('div');
Array.isArray(divElements);// false

var domNodes = Array.prototype.slice.call(document.getElementsByTagName('div'));// apply is ok
Array.isArray(domNodes);// true
```
Javascript 中存在一种名为伪数组的对象结构。比较特别的是 `arguments` 对象，还有像调用 `getElementsByTagName , document.childNodes` 之类的，它们返回 `NodeList` 对象都属于伪数组。不能应用 	`Array` 下的 `push` , `pop` 等方法。

但是我们能通过 `Array.prototype.slice.call` 转换为真正的数组的带有 length 属性的对象，这样 `domNodes` 就可以应用 `Array` 下的所有方法了。

### 3. 深入理解运用 apply、call

下面就借用一道面试题，来更深入的去理解下 apply 和 call 。
```javascript
// 定义一个 log 方法，让它可以代理 console.log 方法，常见的解决方法是：
function log(msg)　{
  console.log(msg);
}
log(1);    //1
log(1,2);    //1
```
上面方法可以解决最基本的需求，但是当传入参数的个数是不确定的时候，上面的方法就失效了，这个时候就可以考虑使用 apply 或者 call，注意这里传入多少个参数是不确定的，所以使用 `apply` 是最好的，方法如下：

```javascript
function log(){
  console.log.apply(window.console, arguments);// 这里的第一次参数就是 consle 自身
};
log(1);    //1
log(1,2);    //1 2
```
接下来的要求是给每一个 log 消息添加一个'(app)'的前辍，比如：

log('hello world');    //(app)hello world

该怎么做比较优雅呢?这个时候需要想到 `arguments` 参数是个伪数组，通过 `Array.prototype.slice.call` 转化为标准数组，再使用数组方法 `unshift`，像这样：

```javascript
function log(){
  var args = Array.prototype.slice.call(arguments);
  args.unshift('(app)');// unshift 在数组的前面添加数据
  
  console.log.apply(console, args);
};
```

## 二、bind

说完了 `apply` 和 `call` ，再来说说 `bind`。
`bind()` 方法与 `apply` 和 `call` 很相似，也是可以改变函数体内 `this` 的指向。

> MDN 的解释是：`bind()` 方法会创建一个 **新函数**，称为绑定函数，当调用这个绑定函数时，绑定函数会以创建它时传入 `bind()` 方法的 **第一个参数** 作为 `this`，传入 `bind()` 方法的 **第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数**。
直接来看看具体如何使用，在常见的单体模式中，通常我们会使用 `_this`, `that` , `self` 等保存 `this` ，这样我们可以在改变了上下文之后继续引用到它。 像这样：

```javascript
var foo = {
	bar : 1,
	eventBind: function(){
	  var _this = this;
	  $('.someClass').on('click',function(event) {
	      /* Act on the event */
	      console.log(_this.bar);     //1
	  });
	}
}
```
由于 Javascript 特有的机制，上下文环境在 `eventBind:function(){ }` 过渡到 `$('.someClass').on('click',function(event) { })` 发生了改变，上述使用变量保存 `this`这些方式都是有用的，也没有什么问题。当然使用 `bind()` 可以更加优雅的解决这个问题：

```javascript
var foo = {
  bar : 1,
  eventBind: function(){
    $('.someClass').on('click',function(event) {
        /* Act on the event */
        console.log(this.bar);      //1
    }.bind(this));
  }
}
```
在上述代码里，`bind()` 创建了一个函数，当这个 `click` 事件绑定在被调用的时候，它的 `this` 关键词会被设置成被传入的值（这里指调用 `bind()` 时传入的参数）。因此，这里我们传入想要的上下文 `this`(其实就是 `foo` )，到 `bind()` 函数中。然后，当回调函数被执行的时候， `this` 便指向 `foo` 对象。再来一个简单的例子：

```javascript
var bar = function(){
	console.log(this.x);
}
var foo = {
	x:3
}
bar(); // undefined
var func = bar.bind(foo);
func(); // 3
```

这里我们创建了一个新的函数 `func`，当使用 `bind()` 创建一个绑定函数之后，它被执行的时候，它的 `this` 会被设置成 `foo` ， 而不是像我们调用 `bar()` 时的 **全局** 作用域。

有个有趣的问题，如果连续 `bind()` 两次，亦或者是连续 `bind()` 三次那么输出的值是什么呢？像这样：

```javascript
var bar = function(){
  console.log(this.x);
}
var foo = {
  x:3
}
var sed = {
  x:4
}
var func = bar.bind(foo).bind(sed);
func(); //?
  
var fiv = {
  x:5
}
var func = bar.bind(foo).bind(sed).bind(fiv);
func(); //?
```
答案是，两次都仍将输出 3 ，而非期待中的 4 和 5 。原因是，在 Javascript 中，多次 `bind()` 是无效的。更深层次的原因， `bind()` 的实现，相当于使用函数在内部包了一个 `call / apply` ，第二次 `bind()` 相当于再包住第一次 `bind()` ,故第二次以后的 `bind` 是无法生效的。

### 其它实例应用:
#### 1. 创建绑定函数
`bind()` 最简单的用法是创建一个函数，使这个函数不论怎么调用都有同样的 `this` 值。JavaScript 新手经常犯的一个错误是将一个方法从对象中拿出来，然后再调用，希望方法中的 this 是原来的对象。（比如在回调中传入这个方法。）如果不做特殊处理的话，一般会丢失原来的对象。从原来的函数和原来的对象创建一个绑定函数，则能很漂亮地解决这个问题：
```javascript
this.x = 9; 
var module = {
  x: 81,
  getX: function() { return this.x; }
};

module.getX(); // 返回 81

var retrieveX = module.getX;// 等价于：var retrieveX = function() { return this.x; };
retrieveX(); // 返回 9, 在这种情况下，'this' 指向全局作用域

// 创建一个新函数，将 'this' 绑定到 `module` 对象
// 新手可能会被全局的 `x` 变量和 `module` 里的属性 `x` 所迷惑
var boundGetX = retrieveX.bind(module);
boundGetX(); // 返回 81
```

#### 2. 分离函数（Partial Functions）
`bind()` 的另一个最简单的用法是使一个函数拥有 **预设的初始参数**。这些参数（如果有的话）作为 `bind()` 的第二个参数跟在 `this`（或其他对象）后面，之后它们会被插入到目标函数的参数列表的 **开始** 位置，传递给绑定函数的参数会跟在它们的后面。
```javascript
function list() {
  return Array.prototype.slice.call(arguments);
}

var list1 = list(1, 2, 3); // [1, 2, 3]

// Create a function with a preset leading argument
var leadingThirtysevenList = list.bind(undefined, 37,'name');
// 上面的第二个参数 37 就是新函数 leadingThirtysevenList 预设的初始参数

var list2 = leadingThirtysevenList(); // [37]
var list3 = leadingThirtysevenList(1, 2, 3); // [37, 1, 2, 3]
```

#### 3. 配合 setTimeout
在默认情况下，使用 `window.setTimeout()` 时，`this` 关键字会指向 `window` （或全局）对象。当使用类的方法时，需要 `this` 引用类的实例，你可能需要显式地把 `this` 绑定到回调函数以便继续使用实例。
```javascript
// 定义一个构造函数
function LateBloomer() {
  this.petalCount = Math.ceil(Math.random() * 12) + 1;// petal: 美 [ˈpɛtl] 花瓣
}
// 在原型链上定义 开花 方法
LateBloomer.prototype.declare = function() {
  console.log('I am a beautiful flower with ' +
    this.petalCount + ' petals!');
};

// 定义了一个过一秒开花的方法
LateBloomer.prototype.bloom = function() {
  // 注意这里 setTimeout 中用到了 bind 方法
  window.setTimeout(this.declare.bind(this), 1000);
};
// 实例化一个对象
var flower = new LateBloomer();
flower.bloom();  // 一秒钟后, 调用 'declare' 方法
```

## 三、apply、call、bind 比较

那么 `apply、call、bind` 三者相比较，之间又有什么异同呢？何时使用 `apply、call`，何时使用 `bind` 呢。简单的一个例子：

```javascript
var obj = {
  x: 81,
};
  
var foo = {
  getX: function() {
    return this.x;
  }
}
console.log(foo.getX.bind(obj)());  //81
console.log(foo.getX.call(obj));    //81
console.log(foo.getX.apply(obj));   //81
```

三个输出的都是 81，但是注意看使用 bind() 方法的，他后面多了对 **括号**。

也就是说，区别是，当你希望改变上下文环境之后并非 **立即执行**，而是回调执行的时候，使用 `bind()` 方法。而 `apply/call` 则会立即执行函数。

再总结一下：
- `apply 、 call 、bind` 三者都是用来改变函数的 `this` 对象的指向的；
- `apply 、 call 、bind` 三者第一个参数都是 `this` 要指向的对象，也就是想指定的上下文；
- `apply 、 call 、bind` 三者都可以利用后续参数传参；
- bind 是返回对应 **函数**，便于稍后调用；`apply 、call` 则是立即调用 。

## 参考：
- [深入浅出 妙用Javascript中apply、call、bind](http://www.admin10000.com/document/6711.html)
- [Function.prototype.bind()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)
- [Function.prototype.call()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
- [Function.prototype.apply()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)