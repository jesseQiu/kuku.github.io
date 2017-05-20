---
title: prototype 与 __proto__ 详解
date: 2016-12-12 15:59:38
updated: 2016-12-12 15:59:38
tags: OOP
categories: JavaScript
feature:
---

## 一、一切皆为对象

> 殊不知，JavaScript的世界中的对象，追根溯源来自于一个 null

在JavaScript中，null也是作为一个对象存在，基于它继承的子子孙孙，当属对象。

乍一看，null像是上帝,而Object和Function犹如JavaScript世界中的亚当与夏娃。

## 二、 原型指针 __proto__

在JavaScript中，每个对象都拥有一个原型对象，而指向该原型对象的内部指针则是__proto__,通过它可以从中继承原型对象的属性，原型是JavaScript中的基因链接，有了这个，才能知道这个对象的祖祖辈辈。从对象中的__proto__可以访问到他所继承的原型对象。

```js
var a = new Array();   
a.__proto__ === Array.prototype // true
```

上面代码中，创建了一个Array的实例a，该实例的原型指向了Array.prototype。  

Array.prototype本身也是一个对象，也有继承的原型:

```js
a.__proto__.__proto__ === Object.prototype  // true   
// 等同于 Array.prototype.__proto__ === Object.prototype
```

这就说了明了，Array本身也是继承自Object的，那么Object的原型指向的是谁呢？  

```js
a.__proto__.__proto__.__proto__ === null  // true   
// 等同于 Object.prototype.__proto__ === null
```

![](http://od6sd4xau.bkt.clouddn.com/prototype-01.png)  

所以说，JavaScript中的对象，追根溯源都是来自一个null对象。佛曰：万物皆空，善哉善哉。

除了使用.__proto__方式访问对象的原型，还可以通过Object.getPrototypeOf方法来获取对象的原型，以及通过Object.setPrototypeOf方法来重写对象的原型  
。

值得注意的是，按照语言标准，__proto__属性只有浏览器才需要部署，其他环境可以没有这个属性，而且前后的两根下划线，表示它本质是一个内部属性，不应该对使用者暴露。因此，应该尽量少用这个属性，而是用Object.getPrototypeof和Object.setPrototypeOf，进行原型对象的读写操作。  
这里用__proto__属性来描述对象中的原型，是因为这样来得更加形象，且容易理解。

## 三、原型对象 prototype

函数作为JavaScript中的一等公民，它既是函数又是对象，函数的原型指向的是Function.prototype

```js
var Foo = function() {}   
Foo.__proto__ === Function.prototype // true
```

函数实例除了拥有__proto__属性之外，还拥有prototype属性。  
通过该函数构造的新的实例对象，其原型指针__proto__会指向该函数的prototype属性。

```js
var a = new Foo();   
a.__proto__ === Foo.prototype; // true
```

而函数的prototype属性，本身是一个由Object构造的实例对象。  

```js
Foo.prototype.__proto__ === Object.prototype; // true
```

prototype属性很特殊，它还有一个隐式的constructor，指向了构造函数本身。  

```js
Foo.prototype.constructor === Foo; 
// truea.constructor === Foo; 
// truea.constructor === Foo.prototype.constructor; 
// true
```

![](http://od6sd4xau.bkt.clouddn.com/prototype-02.png)  


## 四、原型链

### 1. 概念

原型链作为实现继承的主要方法，其基本思想是利用原型让一个引用类型继承另一个引用类型的属性和方法。

每个构造函数都有一个原型对象(prototype)，原型对象都包含一个指向构造函数的指针(constructor)，而实例都包含一个指向原型对象的内部指针(__proto__)。

那么，假如我们让原型对象等于另一个类型的实例，此时的原型对象将包含一个指向另一个原型的指针，相应地，另一个原型中也包含着一个指向另一个构造函数的指针。假如另一个原型又是另一个类型的实例，那么上述关系依然成立。

如此层层递进，就构造了实例与原型的链条，这就是原型链的基本概念。

### 2.意义

“原型链”的作用在于，当读取对象的某个属性时，JavaScript引擎先寻找对象本身的属性，如果找不到，就到它的原型去找，如果还是找不到，就到原型的原型去找。以此类推，如果直到最顶层的Object.prototype还是找不到，则返回undefine


## 五、亲子鉴定

在JavaScript中，也存在鉴定亲子之间DNA关系的方法：

1、instanceof  

运算符返回一个布尔值，表示一个对象是否由某个构造函数创建。

2、Object.isPrototypeOf()  
只要某个对象处在原型链上，isProtypeOf都返回true

```js
var Bar = function() {}   
var b = new Bar();   
b instanceof Bar // true   
Bar.prototype.isPrototypeOf(b) 
// true   Object.prototype.isPrototypeOf(Bar) 
// true
```

要注意，实例b的原型是Bar.prototype而不是Bar  

## 六、一张历史悠久的图

![](http://od6sd4xau.bkt.clouddn.com/prototype-03.jpg)  

这是一张描述了Object、Function以及一个函数实例Foo他们之间原型之间联系。如果理解了上面的概念，这张图是不难读懂。

从上图中，能看到一个有趣的地方。

Function.prototype.__proto__ 指向了 Object.prototype，这说明Function.prototype 是一个 Object实例，那么应当是先有的Object再有Function。

但是Object.prototype.constructor.__proto__ 又指向了 Function.prototype。这样看来，没有Function，Object也不能创建实例。

这就产生了一种类「先有鸡还是先有蛋」的经典问题，到底是先有的Object还是先有的Function呢？

## 参考
- [JavaScript 原型中的哲学思想](http://mp.weixin.qq.com/s?__biz=MzAwNDcyNjI3OA==&mid=2650839267&idx=1&sn=fbe832afcb596858d6d86dcc141fffe2&scene=0#rd)