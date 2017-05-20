---
title: VanillaJs
date: 2016-10-09 13:48:50
updated: 2016-10-09 13:48:50
tags:
categories: JavaScript
feature:
---
![VanillaJs](http://od6sd4xau.bkt.clouddn.com/h5/vanillajs.png)

上图是一张框架性能对比图，如果细心的会发现用红色框起来的 `VanillaJs` ，那这个是什么框架？下面为你逐步解答。

### 一、stackoverflow 上的提问
> What is VanillaJS?

>> I have one simple question, that got stuck in my mind for a few days: What is VanillaJS? Some people refer to it as a framework, you can download a library from the official pages.

>> But when I check some examples or TodoMVC, they just use classic raw JavaScript functions without even including the library from the official pages or anything. Also the link "Docs" on the official webpage leads to the Mozilla specification of JavaScript.

>> My question is: Is VanillaJS raw JavaScript? And if yes, why people refer to it as "framework" when all you need is a browser without any special included scripts?

>> I am sorry for a probably stupid question but I have no idea what people are talking about when they say "VanillaJS".

### 二、官网说明

> The Vanilla JS team maintains every byte of code in the framework and works hard each day to make sure it is small and intuitive. Who's using Vanilla JS? Glad you asked! Here are a few:

> Facebook	Google	YouTube	Yahoo	Wikipedia	Windows Live	Twitter	Amazon	LinkedIn	MSN
eBay	Microsoft	Tumblr	Apple	Pinterest	PayPal	Reddit	Netflix	Stack Overflow
In fact, Vanilla JS is already used on more websites than jQuery, Prototype JS, MooTools, YUI, and Google Web Toolkit - combined.

看了官网说明是不是很冲动，怎么早没用这个框架呢？

### 三、VanillaJs 和 jQuery 例子
- Fade an element out and then remove it
```javascript
// vanillajs
var s = document.getElementById('thing').style;
s.opacity = 1;
(function fade(){(s.opacity-=.1)<0?s.display="none":setTimeout(fade,40)})();

// jQuery	
<script src="//ajax.googleapis.com/ajax/libs/jquery/1/jquery.min.js"></script>
<script>
$('#thing').fadeOut();
</script>
```
- Make an AJAX call
```javascript
// vanillajs
var r = new XMLHttpRequest();
r.open("POST", "path/to/api", true);
r.onreadystatechange = function () {
  if (r.readyState != 4 || r.status != 200) return;
  alert("Success: " + r.responseText);
};
r.send("banana=yellow");

// jQuery	
<script src="//ajax.googleapis.com/ajax/libs/jquery/1/jquery.min.js"></script>
<script>
$.ajax({
  type: 'POST',
  url: "path/to/api",
  data: "banana=yellow",
  success: function (data) {
    alert("Success: " + data);
  },
});
</script>
```
看了上面官网的例子，是不是觉得很熟，不就是原生 `javascript` 码？

### 四、VanillaJs 真身?
带着上面第 3 点留下的疑问，继续解密。
在 `stackoverflow` 上找到了解答: *The plain and simple answer is yes, VanillaJS === JavaScript, as prescribed by Dr B. Eich*，为了进一步确认，找到了一个视频 [What is Vanilla JS?](https://www.youtube.com/watch?v=-OqZzV__hts)。现在可以很确认 `VanillaJS === JavaScript`,至于它的 [vanillajs 官网](http://vanilla-js.com/) 只是以一种轻松搞怪的方式介绍，不要被误导了，不行可以下载 Vanillajs 的库，其实就是一个空文件。

#### 参考
- [stackoverflow 相关解答](http://stackoverflow.com/questions/20435653/what-is-vanillajs)
- [vanillajs *官网*](http://vanilla-js.com/)

