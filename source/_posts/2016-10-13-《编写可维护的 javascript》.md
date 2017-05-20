---
title: 《编写可维护的 javascript》
date: 2016-10-13 16:01:07
updated: 2016-10-13 16:01:07
tags: 
categories: JavaScript
feature:
---

![编写可维护的 javascript](http://od6sd4xau.bkt.clouddn.com/read/maintaintable_javascript.gif)

> 本文主要是我对《编写可维护的 javascript》美 nicbolas c.zakas 著 李晶 郭凯 张散集 译 人民邮电出版社
的读书笔记，以便查阅。
 
1. 行缩进

     使用 4 个空格，不使用 Tab 键，因为不同的编辑器 Tab 键设置不同。
     
2. 语句结尾

    语句结尾需加 ";"。虽然 js 分析器(ASI)会自动插入，但是为了避免分析器自动加入产生歧义。
    
3. 行长度

    80 个字符。
    
4. 换行

    在运算符后换行(ASI 不会再运算符后自动插入";")，下一行增加两个层级缩进(8 个空格)。
    
5. 命名

    变量：以名词为前缀。(小驼峰)
    函数：以动词为前缀。(小驼峰)
    常量：大写字母和下划线。
    构造函数：名词。(大驼峰)
    
6. 字符串

    统一使用双引号("")。各种编程语言可以通用，也使代码风格保持统一。
    ps：如果一串字符串过长，需要换行，使用 "+" 连接。
    
7. null 

    1). 应当使用情形：
      1). 用来初始化一个变量，这个变量可能赋值为一个对象。
      2). 用来和一个已经初始化的变量比较，这个变量可以是也可以不是一个对象。
      3). 当函数参数期望是对象时，用作参数传入。
      4). 当函数的返回值期望是对象时，用作返回值传出。
      
    2).不应当使用情形：
      1). 不要使用 null 来检测是否传入了某个参数。
      2). 不要用 null 检测一个未初始化的变量。
   
   ps：可以把 null 理解为对象的占位符。
   注意和 undefined 区分。(我们应该禁止 undefined 的使用)
   
8. 对象和数组采用直接量方式

	- 对象：
		```javascript
		  var book ={
		      title: "jsvascript",
		      author: "jasin"
		  };
		```
	- 数组：
		```javascript
		  var colors = ["red","green","blue"];
		  var numbers = [1, 2, 3, 4, 5];
		```
9. 注释

	- 单行注释：
	    - // 前后个留个空格。
	    - 可放在被注释语句上面和句尾。如果在上面，保持和被注释语句相同缩进。

	- 多行注释：
	    ```javascript
	      /*
	       * ... ...
	       * ... ...
	      */
	    ```
	- 文档注释：
	    ```javascript
	      /**
	       * 通过 app 名称 判断该 app 进程 id 是否有效
	       * @brief MultiAppManager::checkAppProcessIdByKey
	       * @param dlfKey
	       * @return:true 有效，false:无效
	       **/
	    ```

10. 禁止使用 with 语句。

11. for-in

	for-in 是用来遍历 **对象** 的属性，并返回 **属性的名** 而不是值。它不仅会遍历对象的实例属性，同样还会遍历原型继承来的属性。
	所以我们在遍历实例对象属性时，最好使用 `hasOwnProperty()` 方法来为 for-in 循环过滤出实例属性，避免因为意外的结果而终止。
	例如:

	```javascript
	var prop;

	for (prop in object) {

	// 使用 hasOwnProperty() 方法判断实例对象属性
	if (object.hasOwnProperty(prop)){
	console.log("Property name is " + prop);
	console.log("Property value is " + object[prop]);
	}
	}
	ps：强调一点，for-in 是用来遍历 对象 的属性，所以不能用来遍历数组成员.
	```

12. 严格模式

	建议在函数内部使用 "use strict";因为在全局使用严格模式，则所有函数都是严格模式。如果在和其它非严格模式的文件合并，会统一
	按照严格模式处理，这样有些代码在严格模式下会出错。
    
13. === 和 !==

    在判断相等或不相等，必须使用 === 和 !== 。避免判断的数据内部类型转换，违背本意。
    
14. 禁止使用 eval()

15. 禁止使用原始包装类型。

    例如：
    ```javascript
    var name =  new String("nicholas");
    var name =  new Boolean(true)
    var name =  new Numbers(10)
    ```
    主要是由于开发者会在对象和原始值之间跳来跳去，避免出错几率。
    
16. UI 层松耦合

    HTML、CSS、JavaScript 代码各自独立，避免相互包含，这样可以避免出现问题时难定位。
    
17. 全局变量

    1). 避免使用全局变量。(会带来命名冲突)
    2). 避免使用未声明的变量。(因为这样就创造一个全局变量)
    
      例如：
      ```javascript
        function doSomething(){
            var count = 10;
            name = "nicholas"; // 不好写法，创建了一个全局变量。
        }
      ```
      最好的规则是总是使用 var 来定义变量，哪怕是定义全局变量。
      
    3). 单全局变量方式。// page：75
    4). 模块方式。 // page：78
    5). 零全局变量方式，可以使用地方很局限，实际项目中少用到。
    
18. 事件处理

	- 隔离应用逻辑。
	- 不要分发事件对象。
		例如：
		// 事件处理程序
		```javascript
		var MyApplication = {

		handleClick: function(event){

		    // 假设事件支持 DOM Level2
		    // 下面两句阻止了事件继续冒泡，过滤无用事件信息
		    event.preventDefault();
		    event.stopPropagation();
		    
		    // 传入应用逻辑，提取出事件有用的数据
		    // 这里是事件处理和应用逻辑分界点，这样还可以方便编写测试代码
		    this.showPopup(event.clientX,event.clientY);
		},

		// 应用逻辑处理函数
		showPopup: function(x,y){
		    var popup = document.getElementById("popup");
		    popup.style.left = x + "px";
		    popup.style.top = y + "px";
		    popup.className = "reveal";

		}
		};

		// 绑定触发元素
		addListener(element,"click",function(event){
		MyApplication.handleClick(event);
		});
		```
19. 检测原始值

	js 包含 5 种原始值：字符串、数字、布尔值、null、undefined。

	检测方法：
	```javascript
	// 检测字符串
	if(typeof name === "string") {
	  // ...
	}

	// 检测数字
	if(typeof count === "number") {
	  // ...
	}

	// 检测布尔值
	if(typeof found === "boolean" && found) {
	  // ...
	}

	// 检测 undefined
	if(typeof MyApp === "undefined") {
	  // ...
	}
	```
	null 一般不用于检测语句，typeof null 返回 "object"。但是有个例外，如果期望的值真是 null，可以用该值比较。

	例如：
	```javascript
	var element = document.getElementById("my-div");// 如果 DOM 元素不存在 返回 null，反之返回一个节点。
	if (element !== null){
	    element.className = "found";
	}
	```
20. 检测引用值(对象 object)

    除了原始值外，还有几种内置应用类型：object、array、date 和 error。
    
    不能使用 typeof 因为都是返回 "object"，应该使用 instanceof。
    
      例如：
      ```javascript
        var now = new Date();
        now instanceof Object  // true 所有的对象都认为是 object 的实例
        now instanceof Date    // true
     ```
    同样可以检测自定义对象。
    
    注意：instance 检测不能跨帧(frame)。

21. 检测函数

	使用 typeof
	例如：
	```javascript
	function myFunction(){};
	typeof myFunction === "function"; // true
	```
	注意：检测函数可以跨帧 (frame)。

22. 检测数组
	```javascript
	function isArray(value){
	    // 先判断是否有 isArray 函数，因为该函数是在 ECMAScript5 引入
	    if(typeof Array.isArray === "function"){
	        return Array.isArray(value);
	    }else{
	        // 如果没有 isArray 函数则使用该方法，因为用内置 toString 在所有浏览器中都会返回标准字符串
	        // 对于数组返回 "[object Array]"
	        return Object.prototype.toString.call(value) === "[object Array]";
	    }
	}
	```
	注意该方法支持跨域

23. 检测对象属性

    1). 判断属性最好的方法是使用 in 运算符。in 运算符仅仅会简单的判断属性值是否存在，而不会去读属性的值。
    
      例如：
      ```javascript
        var Object = {
            count: 0,
            related: null
        };
        
        if("count" in Object){ // true
            // ...
        }
    	```
    2). 如果想检测某个实例对象的属性使用 object.hasOwnProperty()，所以继承 Object 都有该方法
    
      例如：
      ```javascript
        if (object.hasOwnProperty("related")){ // true 
            // ...
        }
      ```
    3). 如果是 DOM 对象，需要增加该方法的判断，因为 IE8 及更早版本 IE 中， DOM 对象并非继承自 Object。
    
      例如：
      ```javascript
        if ("hasOwnProperty" in object && object.hasOwnProperty("related")){ // true 
            // ...
        }
      ```
24. 错误类型
    
    ECMA-262 规范指出 7 中错误类型：
    
    1). Error、EvalError、RangeError、ReferenceError、SyntaxError、SyntaxError、TypeError、URIError // 详细参阅 page：109
    
      例如：
      ```javascript
        tyr{
            // 可能引发错误的代码
        }catch(ex){
            if(ex instanceof TypeError){
                // ...
            }else if(ex instanceof ReferenceError){
                // ...
            }else{
                // ...
            }
        }
      ```
    2). 错误类型继承
    
      例如：
      ```javascript
        function MyError(msg){
            this.msg = msg;
        }
        MyError.prototype = new Error();
    ```
    3). 抛出自定义异常
    
      例如：
      ```javascript
        throw new MyError("hello world!!!");
      ``` 
    4). 检测自定义异常
    
      例如：
      ```javascript
          tyr{
              // 可能引发错误的代码
          }catch(ex){
              if(ex instanceof MyError){
                  // ...
              }else{
                  // ...
              }
          }
      ```
25. 基于对象继承和基于类型继承

	1). 基于对象继承
	例如：
	```javascript
	var person = {
	name: "nike",
	sayName:function(){
	alert(this.name);
	}
	};

	var myPerson = Object.create(person);
	myPerson.sayName(); // 弹出 "nike"
	```
	2). 基于类型继承
	例如：
	```javascript
	function MyError(msg){
	this.msg = msg;
	}
	MyError.prototype = new Error();
	```
