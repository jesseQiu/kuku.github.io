---
title: ES6 Dtudy Notes
tags:
---

> 本文主要是记录 ES6 的学习过程中的知识点，帮助以后回顾总结。学习的课程是 [阮一峰的ES6 入门](http://es6.ruanyifeng.com/#docs/intro)。

1. ECMAScript 和 JavaScript 的关系
	一个常见的问题是，ECMAScript 和 JavaScript 到底是什么关系？
	要讲清楚这个问题，需要回顾历史。1996年11月，JavaScript的创造者Netscape公司，决定将JavaScript提交给国际标准化组织ECMA，希望这种语言能够成为国际标准。次年，ECMA发布262号标准文件（ECMA-262）的第一版，规定了浏览器脚本语言的标准，并将这种语言称为ECMAScript，这个版本就是1.0版。
	该标准从一开始就是针对JavaScript语言制定的，但是之所以不叫JavaScript，有两个原因。一是商标，Java是Sun公司的商标，根据授权协议，只有Netscape公司可以合法地使用JavaScript这个名字，且JavaScript本身也已经被Netscape公司注册为商标。二是想体现这门语言的制定者是ECMA，不是Netscape，这样有利于保证这门语言的开放性和中立性。
	因此，ECMAScript和JavaScript的关系是，前者是后者的规格，后者是前者的一种实现（另外的ECMAScript方言还有Jscript和ActionScript）。日常场合，这两个词是可以互换的。

2. ECMAScript 的历史
	ES6从开始制定到最后发布，整整用了15年。
	前面提到，ECMAScript 1.0是1997年发布的，接下来的两年，连续发布了ECMAScript 2.0（1998年6月）和ECMAScript 3.0（1999年12月）。3.0版是一个巨大的成功，在业界得到广泛支持，成为通行标准，奠定了JavaScript语言的基本语法，以后的版本完全继承。直到今天，初学者一开始学习JavaScript，其实就是在学3.0版的语法。
	2000年，ECMAScript 4.0开始酝酿。这个版本最后没有通过，但是它的大部分内容被ES6继承了。因此，ES6制定的起点其实是2000年。
	为什么ES4没有通过呢？因为这个版本太激进了，对ES3做了彻底升级，导致标准委员会的一些成员不愿意接受。ECMA的第39号技术专家委员会（Technical Committee 39，简称TC39）负责制订ECMAScript标准，成员包括Microsoft、Mozilla、Google等大公司。
	2007年10月，ECMAScript 4.0版草案发布，本来预计次年8月发布正式版本。但是，各方对于是否通过这个标准，发生了严重分歧。以Yahoo、Microsoft、Google为首的大公司，反对JavaScript的大幅升级，主张小幅改动；以JavaScript创造者Brendan Eich为首的Mozilla公司，则坚持当前的草案。
	2008年7月，由于对于下一个版本应该包括哪些功能，各方分歧太大，争论过于激烈，ECMA开会决定，中止ECMAScript 4.0的开发，将其中涉及现有功能改善的一小部分，发布为ECMAScript 3.1，而将其他激进的设想扩大范围，放入以后的版本，由于会议的气氛，该版本的项目代号起名为Harmony（和谐）。会后不久，ECMAScript 3.1就改名为ECMAScript 5。
	2009年12月，ECMAScript 5.0版正式发布。Harmony项目则一分为二，一些较为可行的设想定名为JavaScript.next继续开发，后来演变成ECMAScript 6；一些不是很成熟的设想，则被视为JavaScript.next.next，在更远的将来再考虑推出。TC39委员会的总体考虑是，ES5与ES3基本保持兼容，较大的语法修正和新功能加入，将由JavaScript.next完成。当时，JavaScript.next指的是ES6，第六版发布以后，就指ES7。TC39的判断是，ES5会在2013年的年中成为JavaScript开发的主流标准，并在此后五年中一直保持这个位置。
	2011年6月，ECMAscript 5.1版发布，并且成为ISO国际标准（ISO/IEC 16262:2011）。
	2013年3月，ECMAScript 6草案冻结，不再添加新功能。新的功能设想将被放到ECMAScript 7。
	2013年12月，ECMAScript 6草案发布。然后是12个月的讨论期，听取各方反馈。
	2015年6月，ECMAScript6正式通过，成为国际标准。从2000年算起，这时已经过去了15年。

3. [各大浏览器对 ES6 的支持情况](http://kangax.github.io/compat-table/es6/)

4. [Babel 转码器](https://babeljs.io/)
	Babel是一个广泛使用的ES6转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。这意味着，你可以用ES6的方式编写程序，又不用担心现有环境是否支持。下面是一个例子。

	```javascript
	// 转码前
	input.map(item => item + 1);

	// 转码后
	input.map(function (item) {
	  return item + 1;
	});
	```

	上面的原始代码用了箭头函数，这个特性还没有得到广泛支持，Babel将其转为普通函数，就能在现有的JavaScript环境执行了。

5. [Traceur 转码器](https://github.com/google/traceur-compiler)
	Google 公司的 Traceur 转码器，也可以将 ES6 代码转为 ES5 代码。

6. let 命令
	ES6 新增了 let 命令，用来声明变量。它的用法类似于 var，但是所声明的变量，只在 let 命令所在的代码块内有效
	```javascript
	for (let i = 0; i < arr.length; i++) {}
	console.log(i);
	//ReferenceError: i is not defined
	```
	声明的变量不是全局对象的属性，同样的 const 也一样。

7. const 命令
	const 声明一个只读的常量。一旦声明，常量的值就不能改变
	对于复合类型的变量，变量名不指向数据，而是指向数据所在的地址。const 命令只是保证变量名指向的地址不变，并不保证该地址的数据不变，所以将一个对象声明为常量必须非常小心。
	```javascript
	const foo = {};
	foo.prop = 123;

	foo.prop
	// 123

	foo = {}; // TypeError: "foo" is read-only
	```

	如果真的想将对象冻结，应该使用Object.freeze方法。
	```javascript
	const foo = Object.freeze({});

	// 常规模式时，下面一行不起作用；
	// 严格模式时，该行会报错
	foo.prop = 123;
	```
	将对象彻底冻结的函数。
	```javascript
	var constantize = (obj) => {
	  Object.freeze(obj);
	  Object.keys(obj).forEach( (key, value) => {
	    if ( typeof obj[key] === 'object' ) {
	      constantize( obj[key] );
	    }
	  });
	};
	```

8. 变量的解构赋值
	- 数组的解构赋值
		基本用法:
		```javascript
		var [a, b, c] = [1, 2, 3];
		```
		默认值:
		注意，ES6内部使用严格相等运算符（===），判断一个位置是否有值。所以，如果一个数组成员不严格等于undefined，默认值是不会生效的。
		```javascript
			var [x = 1] = [undefined];
			x // 1

			var [x = 1] = [null];
			x // null
		```
		上面代码中，如果一个数组成员是null，默认值就不会生效，因为null不严格等于undefined

	- 对象的解构赋值
		```javascript
			var { foo, bar } = { foo: "aaa", bar: "bbb" };
			foo // "aaa"
			bar // "bbb"
		```
		对象的解构赋值是下面形式的简写
		```javascript
			var { foo: foo, bar: bar } = { foo: "aaa", bar: "bbb" };
		```
	- 字符串的解构赋值
		字符串也可以解构赋值。这是因为此时，字符串被转换成了一个类似数组的对象
		```javascript
			const [a, b, c, d, e] = 'hello';
			a // "h"
			b // "e"
			c // "l"
			d // "l"
			e // "o"
		```
	- 用途
		- 交换变量的值
		```javascript
			[x, y] = [y, x];	
		```
		上面代码交换变量x和y的值，这样的写法不仅简洁，而且易读，语义非常清晰。

		- 从函数返回多个值
		函数只能返回一个值，如果要返回多个值，只能将它们放在数组或对象里返回。有了解构赋值，取出这些值就非常方便。
		```javascript
			// 返回一个数组
			function example() {
			return [1, 2, 3];
			}
			var [a, b, c] = example();

			// 返回一个对象

			function example() {
			return {
			foo: 1,
			bar: 2
			};
			}
			var { foo, bar } = example();
		```
		- 函数参数的定义
		解构赋值可以方便地将一组参数与变量名对应起来。
		```javascript
			// 参数是一组有次序的值
			function f([x, y, z]) { ... }
			f([1, 2, 3]);

			// 参数是一组无次序的值
			function f({x, y, z}) { ... }
			f({z: 3, y: 2, x: 1});
		```
		- 提取JSON数据
		解构赋值对提取JSON对象中的数据，尤其有用。
		```javascript
			var jsonData = {
			id: 42,
			status: "OK",
			data: [867, 5309]
			};

			let { id, status, data: number } = jsonData;

			console.log(id, status, number);
			// 42, "OK", [867, 5309]
		```
		上面代码可以快速提取JSON数据的值。

		- 函数参数的默认值
		```javascript
			jQuery.ajax = function (url, {
			async = true,
			beforeSend = function () {},
			cache = true,
			complete = function () {},
			crossDomain = false,
			global = true,
			// ... more config
			}) {
			// ... do stuff
			};
		```
		指定参数的默认值，就避免了在函数体内部再写var foo = config.foo || 'default foo';这样的语句。

		- 遍历Map结构
		任何部署了Iterator接口的对象，都可以用for...of循环遍历。Map结构原生支持Iterator接口，配合变量的解构赋值，获取键名和键值就非常方便。
		```javascript
			var map = new Map();
			map.set('first', 'hello');
			map.set('second', 'world');

			for (let [key, value] of map) {
			console.log(key + " is " + value);
			}
			// first is hello
			// second is world
			如果只想获取键名，或者只想获取键值，可以写成下面这样。

			// 获取键名
			for (let [key] of map) {
			// ...
			}

			// 获取键值
			for (let [,value] of map) {
			// ...
			}
		```
		- 输入模块的指定方法
		加载模块时，往往需要指定输入那些方法。解构赋值使得输入语句非常清晰。
		```javascript
			const { SourceMapConsumer, SourceNode } = require("source-map");
		```

9. 字符串的扩展
	- 字符的 Unicode 表示法
	JavaScript 内部，字符以 UTF-16 的格式储存，每个字符固定为 2 个字节。对于那些需要 4 个字节储存的字符（Unicode码点大于0xFFFF的字符），JavaScript 会认为它们是两个字符
	```javascript
		"\u0061"
		// "a"
		"\uD842\uDFB7"
		// "𠮷"
	```
	如果超过 0xFFFF 字符可以用 {} 表示	
	```javascript
		"\u{20BB7}"
		// "𠮷"

		"\u{41}\u{42}\u{43}"
		// "ABC"

		let hello = 123;
		hell\u{6F} // 123

		'\u{1F680}' === '\uD83D\uDE80'
		// true
	```
	- codePointAt()
	ES6 提供了codePointAt方法，能够正确处理4个字节储存的字符，返回一个字符的码点
	```javascript
		var s = '𠮷a';
		s.codePointAt(0) // 134071
		s.codePointAt(1) // 57271
		s.charCodeAt(2) // 97

		//
		function is32Bit(c) {
		  return c.codePointAt(0) > 0xFFFF;
		}

		is32Bit("𠮷") // true
		is32Bit("a") // false

		// 使用 for of 正确遍历出 4 字节字符
		var s = '𠮷a';
		for (let ch of s) {
		  console.log(ch.codePointAt(0).toString(16));
		}
		// 20bb7
		// 61

	```
	- String.fromCodePoint() 
	ES6提供了String.fromCodePoint方法，可以识别0xFFFF的字符，弥补了String.fromCharCode方法的不足。在作用上，正好与codePointAt方法相反。
	```javascript
		String.fromCodePoint(0x20BB7)
		// "𠮷"
		String.fromCodePoint(0x78, 0x1f680, 0x79) === 'x\uD83D\uDE80y'
		// true
	```
	上面代码中，如果String.fromCodePoint方法有多个参数，则它们会被合并成一个字符串返回。
	** 注意，fromCodePoint方法定义在String对象上，而codePointAt方法定义在字符串的实例对象上**

	- includes(), startsWith(), endsWith() 
	传统上，JavaScript只有indexOf方法，可以用来确定一个字符串是否包含在另一个字符串中。ES6又提供了三种新方法。
		- includes()：返回布尔值，表示是否找到了参数字符串。
		- startsWith()：返回布尔值，表示参数字符串是否在源字符串的头部。
		- endsWith()：返回布尔值，表示参数字符串是否在源字符串的尾部。

	```javascript
		var s = 'Hello world!';
		s.startsWith('Hello') // true
		s.endsWith('!') // true
		s.includes('o') // true
	```

	- repeat()
	repeat方法返回一个新字符串，表示将原字符串重复n次
	```javascript
		'x'.repeat(3) // "xxx"
		'hello'.repeat(2) // "hellohello"
		'na'.repeat(0) // ""
	```
	- padStart()，padEnd()
	**ES7** 推出了字符串补全长度的功能。如果某个字符串不够指定长度，会在头部或尾部补全。padStart用于头部补全，padEnd用于尾部补全。
	```javascript
		'x'.padStart(5, 'ab') // 'ababx'
		'x'.padStart(4, 'ab') // 'abax'

		'x'.padEnd(5, 'ab') // 'xabab'
		'x'.padEnd(4, 'ab') // 'xaba'
	```
	- 模板字符串
	传统的JavaScript语言，输出模板通常是这样写的。
	```javascript
		$('#result').append(
		  'There are <b>' + basket.count + '</b> ' +
		  'items in your basket, ' +
		  '<em>' + basket.onSale +
		  '</em> are on sale!'
		);
	```
	上面这种写法相当繁琐不方便，ES6引入了模板字符串解决这个问题。
	```javascript
		$('#result').append(`
		  There are <b>${basket.count}</b> items
		   in your basket, <em>${basket.onSale}</em>
		  are on sale!
		`);
	```

10. 正则表达式的扩展
	- 正确返回字符串长度,支持大于 `0xFFFF` 的 `Unicode` 字符.
	```javascript
	function codePointLength(text) {
	  var result = text.match(/[\s\S]/gu);
	  return result ? result.length : 0;
	}

	var s = '𠮷𠮷';

	s.length // 4
	codePointLength(s) // 2
	```