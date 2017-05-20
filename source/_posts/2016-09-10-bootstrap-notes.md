---
title: Bootstrap Notes
date: 2016-09-10 11:53:30
updated: 2016-09-10 11:53:30
tags:
categories: Bootstrap
---

### 本文主要是记录在学习和实践 `bootstrap` 过程需要注意的点，具体的教程可以参考 [菜鸟教程](http://www.runoob.com/bootstrap/bootstrap-tutorial.html)

1. 在代码中注意全角和半角，尤其是 **空格** 我们肉眼很难看出差别
	- 全角空格
		```html
			<div class="container">
			  <div　class="jumbotron"> 
			          <h1>欢迎登陆页面！</h1> 
			          <p>这是一个超大屏幕（Jumbotron）的实例。</p> 
			          <p><a class="btn btn-primary btn-lg" role="button">lucky-kuku</a> 
			      </p> 
			  </div> 
			</div>
		```

	- 半角空格
		```javascript
			<div class="container">
			  <div class="jumbotron"> 
			          <h1>欢迎登陆页面！</h1> 
			          <p>这是一个超大屏幕（Jumbotron）的实例。</p> 
			          <p><a class="btn btn-primary btn-lg" role="button">lucky-kuku</a> 
			      </p> 
			  </div> 
			</div>

		```
		** 能看出上面代码差别么？ **
	- 上面代码显示图片
		- 全角空格 不能正常显示 `jumbotron` 的样式
			![全角空格显示](http://od6sd4xau.bkt.clouddn.com/bootstrap/bootstrap-notes-02.png)
		- 半角空格 正常显示 `jumbotron` 的样式
			![半角空格显示](http://od6sd4xau.bkt.clouddn.com/bootstrap/bootstrap-notes-01.png)
	- 全角半角区别
		- 全角输入: 字符占两个字符
		- 半角输入: 字符占一个字符

