---
title: Vue.js Start
date: 2016-09-20 11:26:21
updated: 2016-09-22 11:26:21
tags:
categories: Vue.js
feature:
---

> 本文主要是记录 `Vue.js` 的学习起步和相应的资料整合

### 一、Install

- `Vue.js` 使用 `ECMAScript 5` 特性，不支持 `IE8` 及以下版本浏览器。 
- `Vue.js` 库下载
	- 官方推荐使用 `npm` 方式下载
		```bash
			# latest stable
			$ npm install vue

			# latest stable + CSP-compliant (支持 CSP 安全策略版本)
			$ npm install vue@csp
		```
	- `bower` 方式下载
		```bash
			# latest stable
			$ bower install vue
		```

- `Vue.js` 官方脚手架工具
	```bash
		# install vue-cli
		$ npm install -g vue-cli

		# create a new project using the "webpack" boilerplate
		$ vue init webpack my-project

		# install dependencies and go!
		$ cd my-project
		$ npm install
		$ npm run dev
	```
	[官方脚手架工具链接 vue-cli](https://github.com/vuejs/vue-cli) 


### 二、Overview
1. Reactive Data Binding
	`Vue.js` 是数据驱动视图(data-driven view)机制,如下图
	![vue-mvvm](http://od6sd4xau.bkt.clouddn.com/h5/vue-mvvm.png)

	对应上图各个模块代码

	HTML
	```html
		<!-- this is our View -->
		<div id="app">
			Hello {{ name }}!
		</div>
	```
	JS
	```javascript
		// this is our Model
		var exampleData = {
			name: 'Vue.js'
		}

		// create a Vue instance, or, a "ViewModel"
		// which links the View and the Model
		var exampleVM = new Vue({
			el: '#app',
			data: exampleData
		})
	```
2. Component System
	`Vue.js` 采用组件方式搭建应用
	![vue-component](http://od6sd4xau.bkt.clouddn.com/h5/vue-component.png)

	```html
	<div id="app">
		<app-nav></app-nav>
		<app-view>
			<app-sidebar></app-sidebar>
			<app-content></app-content>
		</app-view>
	</div>
	```

### 三、Vue.js 特点

- 简洁： HTML 模板 + JSON 数据，再创建一个 Vue 实例，就这么简单。
- 数据驱动： 自动追踪依赖的模板表达式和计算属性。
- 组件化： 用解耦、可复用的组件来构造界面。
- 轻量： ~24kb min+gzip，无依赖。
- 快速： 精确有效的异步批量 DOM 更新。
- 模块友好： 通过 NPM 或 Bower 安装，无缝融入你的工作流。
- 如果你喜欢下面这些，那你一定会喜欢 Vue.js：
	- 可扩展的数据绑定机制
	- 原生对象即模型
	- 简洁明了的 API
	- 组件化 UI 构建
	- 多个轻量库搭配使用


---
### 学习资料
- [Vue.js 官网](http://vuejs.org/)
- [Vue.js github](https://github.com/vuejs/vue/)
- [Release Notes](https://github.com/vuejs/vue/releases) *发布版本履历*
- [Vue.js 中文文档](http://cn.vuejs.org/) *官方入门教程*
- [Vue.js 中文社区](http://www.vue-js.com/)
- [Comparison with Other Frameworks](http://vuejs.org/guide/comparison.html#Angular)
- [Building Large-Scale Apps](http://vuejs.org/guide/application.html)
- [vue-devtools](https://github.com/vuejs/vue-devtools) *Vue.js 版的 Todolist 源码*  
- [Vue.js 极客学员教程](http://wiki.jikexueyuan.com/project/vue-js/)
- [菜鸟教程](http://www.runoob.com/w3cnote/vue-js-quickstart.html)
- [Vue.js：轻量高效的前端组件化方案](http://www.csdn.net/article/1970-01-01/2825439)
- [用 Vue.js 制作的 FWA/Awwwards 获奖站点](https://github.com/vuejs/awesome-vue) *里面收集了和 `Vue.js` 相关的网站和 `App`*
- [讲解 Vue.js 官网 中文-含代码、百度云、youtube](https://github.com/bhnddowinf/vuejs-learn) 
- [对比其它框架](http://cn.vuejs.org/guide/comparison.html)
- [官方 2.0 例子源码](https://github.com/vuejs/vue/tree/next/examples) *Vue.js 源码自带的例子*
- [1.0 VS 2.0 特性](https://github.com/vuejs/vue/issues/2873)
- [Vue 2.0 Pre-alpha 版本介绍](https://zhuanlan.zhihu.com/p/20814761) *尤雨溪知乎专栏*
- [Vue.js 基础到进阶教程](https://github.com/keepfool/vue-tutorials) [对应的博客](http://www.cnblogs.com/keepfool/p/5690366.html) *推荐学习*
- [Element](http://element.eleme.io/#/) *一套为开发者、设计师和产品经理准备的基于 Vue 2.0 的组件库*
- [VueStrap](http://yuche.github.io/vue-strap/) *基于 Vue.js 的 bootstrap 组件*
- [Vue.js 最佳实践](http://wiki.jikexueyuan.com/project/vue-js/practices.html)
- [vue webpack 模板说明](http://vuejs-templates.github.io/webpack/)

