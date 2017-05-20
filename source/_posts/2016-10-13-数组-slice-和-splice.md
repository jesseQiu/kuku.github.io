---
title: 数组 Slice() 和 Splice()
date: 2016-10-13 15:14:07
updated: 2016-10-13 15:14:07
tags:
categories: JavaScript
feature:
---

> 在开发中经常碰到 `[].slice()` 和 `[].splice()` 两个方法，但是总是不能很好的区分它们的差别和用法，今天刚好又碰到就查阅相关文档，做个小结，以便加深印象。

### 一、Array.prototype.slice()

slice() 方法会浅复制（shallow copy）数组的一部分到一个新的数组，并返回这个新数组。

1. 单词意思
slice  美 [slaɪs] 
vt.  切成片; 切下; 划分;
n.  薄片; 一部分; （因失误而打出的）曲线球;
vi.  斜击;

2. 语法
arr.slice([begin[, end]])

	- 参数
		- begin
			从该索引处开始提取原数组中的元素（从0开始）。
			如果该参数为负数，则表示从原数组中的倒数第几个元素开始提取，slice(-2)表示提取原数组中的倒数第二个元素到最后一个元素（包含最后一个元素）。
			如果省略 begin，则 slice 从索引 0 开始。

		- end
			在该索引处结束提取原数组元素（从0开始）。slice 会提取原数组中索引从 begin 到 end 的所有元素（包含 begin，但 **不包含 end**）。
			slice(1,4) 提取原数组中的第二个元素开始直到第四个元素的所有元素 （索引为 1, 2, 3的元素）。
			如果该参数为负数， 则它表示在原数组中的倒数第几个元素结束抽取。 slice(-2,-1)表示抽取了原数组中的倒数第二个元素到最后一个元素（不包含最后一个元素，也就是只有倒数第二个元素）。
			如果 end 被省略，则 slice 会一直提取到原数组末尾。
			返回值是一个含有提取元素的新数组

3. 描述
	slice **不修改原数组**，只会返回一个浅复制了原数组中的元素的一个新数组。原数组的元素会按照下述规则拷贝：

	如果该元素是个对象引用 （不是实际的对象），slice 会拷贝这个对象引用到新的数组里。两个对象引用都引用了同一个对象。如果被引用的对象发生改变，则新的和原来的数组中的这个元素也会发生改变。
	对于字符串、数字及布尔值来说（不是 String、Number 或者 Boolean 对象），slice 会拷贝这些值到新的数组里。在别的数组里修改这些字符串或数字或是布尔值，将不会影响另一个数组。
	如果向两个数组任一中添加了新元素，则另一个不会受到影响。

4. 示例

- 返回数组中的一部分
	```javascript
	// Our good friend the citrus from fruits example
	var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
	var citrus = fruits.slice(1, 3); // 注意这里不包含下标 3("Apple") 的元素

	// puts --> ["Orange","Lemon"]
	```

- 使用 slice
	在下例中, slice 从 myCar 中创建了一个新数组 newCar. 两个数组都包含了一个 myHonda 对象的引用. 当 myHonda 的 color 属性改变为 purple, 则两个数组中的对应元素都会随之改变.

	```javascript
	// 使用 slice方法从 myCar 中创建一个 newCar.
	var myHonda = { color: "red", wheels: 4, engine: { cylinders: 4, size: 2.2 } };
	var myCar = [myHonda, 2, "cherry condition", "purchased 1997"];
	var newCar = myCar.slice(0, 2);

	// 输出myCar, newCar,以及各自的myHonda对象引用的color属性.
	print("myCar = " + myCar.toSource());
	print("newCar = " + newCar.toSource());
	print("myCar[0].color = " + myCar[0].color);
	print("newCar[0].color = " + newCar[0].color);

	// 改变myHonda对象的color属性.
	myHonda.color = "purple";
	print("The new color of my Honda is " + myHonda.color);

	//输出myCar, newCar中各自的myHonda对象引用的color属性.
	print("myCar[0].color = " + myCar[0].color);
	print("newCar[0].color = " + newCar[0].color);

	上述代码输出:

	myCar = [{color:"red", wheels:4, engine:{cylinders:4, size:2.2}}, 2, "cherry condition", "purchased 1997"]
	newCar = [{color:"red", wheels:4, engine:{cylinders:4, size:2.2}}, 2]
	myCar[0].color = red 
	newCar[0].color = red
	The new color of my Honda is purple
	myCar[0].color = purple
	newCar[0].color = purple
	```

- 类数组（Array-like）对象
	slice 方法可以用来将一个类数组（Array-like）对象/集合转换成一个数组。你只需将该方法绑定到这个对象上。下述代码中 list 函数中的 arguments 就是一个类数组对象。

	```javascript
	function list() {
	  return Array.prototype.slice.call(arguments);
	}

	var list1 = list(1, 2, 3); // [1, 2, 3]
	除了使用 Array.prototype.slice.call(arguments)，你也可以简单的使用 [].slice.call(arguments) 来代替。另外，你可以使用 bind 来简化该过程。

	var unboundSlice = Array.prototype.slice;
	var slice = Function.prototype.call.bind(unboundSlice);

	function list() {
	  return slice(arguments);
	}

	var list1 = list(1, 2, 3); // [1, 2, 3]
	```


### 二、Array.prototype.splice()

splice() 方法用新元素替换旧元素，以此修改数组的内容。

1. 单词意思
	splice 美 [splaɪs] 
	vt.  绞接; 捻接（两段绳子）; 胶接; 粘接（胶片、磁带等）;
	n.  胶接处，粘接处，铰接处;

2. 语法
	array.splice(start, deleteCount[, item1[, item2[, ...]]])

3. 参数

	- start​
		从数组的哪一位开始修改内容。如果超出了数组的长度，则从数组末尾开始添加内容；如果是负值，则表示从数组末位开始的第几位。

	- deleteCount
		整数，表示要移除的数组元素的个数。如果 deleteCount 是 0，则不移除元素。这种情况下，至少应添加一个新元素。如果 deleteCount 大于 start 之后的元素的总数，则从 start 后面的元素都将被删除（含第 start 位）。

	- itemN
		要添加进数组的元素。如果不指定，则 splice() 只删除数组元素。

	- 返回值
		由被删除的元素组成的一个数组。如果只删除了一个元素，则返回只包含一个元素的数组。如果没有删除元素，则返回空数组。

4. 描述
	如果添加进数组的元素个数不等于被删除的元素个数，数组的长度会发生相应的改变。

5. 提示和注释
	注释：请注意，splice() 方法与 slice() 方法的作用是不同的，splice() 方法会直接对数组进行修改。

6. 示例
	使用 splice()
	```javascript
	如下代码演示了 splice 的用法：

	var myFish = ["angel", "clown", "mandarin", "surgeon"];

	//从第 2 位开始删除 0 个元素，插入 "drum"
	var removed = myFish.splice(2, 0, "drum"); // 这里的 "drum" 是被插入到 下标 2("mandarin") 的前面
	//运算后的 myFish:["angel", "clown", "drum", "mandarin", "surgeon"]
	//被删除元素数组：[]，没有元素被删除

	//从第 3 位开始删除 1 个元素
	removed = myFish.splice(3, 1);
	//运算后的myFish：["angel", "clown", "drum", "surgeon"]
	//被删除元素数组：["mandarin"]

	//从第 2 位开始删除 1 个元素，然后插入 "trumpet"
	removed = myFish.splice(2, 1, "trumpet");
	//运算后的myFish: ["angel", "clown", "trumpet", "surgeon"]
	//被删除元素数组：["drum"]

	//从第 0 位开始删除 2 个元素，然后插入 "parrot", "anemone" 和 "blue"
	removed = myFish.splice(0, 2, "parrot", "anemone", "blue");
	//运算后的myFish：["parrot", "anemone", "blue", "trumpet", "surgeon"]
	//被删除元素的数组：["angel", "clown"]

	//从第 3 位开始删除 2 个元素
	removed = myFish.splice(3, Number.MAX_VALUE);
	//运算后的myFish: ["parrot", "anemone", "blue"]
	//被删除元素的数组：["trumpet", "surgeon"]
	```

### 三、slice Vs splice
1. 作用不同：
	- slice: 浅复制（shallow copy）数组的一部分到一个新的数组，并返回这个新数组
	- splice: 用新元素替换旧元素，以此修改数组的内容
2. 对原数组的影响不同
	- slice: 返回新数组，不影响原数组
	- splice: 会更改原数组值

### 参考
- [Array.prototype.slice()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)
- [Array.prototype.splice()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)