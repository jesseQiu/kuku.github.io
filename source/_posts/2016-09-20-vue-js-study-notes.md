---
title: Vue.js Study Notes
date: 2016-09-20 22:51:57
updated: 2016-09-20 22:51:57
tags:
categories: Vue.js
feature:
---

> 本文主要是记录在学习 `Vue.js` 过程中的知识点。

1. `v-bind` 和 `v-on` 缩写方法
```html
	<!-- 完整语法 -->
	<a v-bind:href="url"></a>
	<!-- 缩写 -->
	<a :href="url"></a>

	<!-- 完整语法 -->
	<button v-bind:disabled="someDynamicCondition">Button</button>
	<!-- 缩写 -->
	<button :disabled="someDynamicCondition">Button</button>

	<!-- 完整语法 -->
	<a v-on:click="doSomething"></a>

	<!-- 缩写 -->
	<a @click="doSomething"></a>
```

2. `Vue.js` 本身不带 **路由** 和 **Ajax** 请求部分，需要使用相应的插件，[vue-router](https://github.com/vuejs/vue-router) 和 [vue-resource](https://github.com/vuejs/vue-resource)。

3. 使用 **计算属性** 替代 `$watch` 代码会更简洁。参考： [计算属性](http://cn.vuejs.org/guide/computed.html)

4. 条件渲染
	- v-show 不支持 <template> 语法。
	- v-show 用在组件上时，因为指令的优先级 v-else 会出现问题，可以改为
		```html
			<custom-component v-show="condition"></custom-component>
			<p v-show="!condition">这可能也是一个组件</p>
		```
	- 如果需要频繁切换 `v-show` 较好，如果在运行时条件不大可能改变 `v-if` 较好。
	- 参考 [条件渲染](http://cn.vuejs.org/guide/conditional.html)

5. 事件
	- 事件修饰符
	```html
		<!-- 阻止单击事件冒泡 -->
		<a v-on:click.stop="doThis"></a>

		<!-- 提交事件不再重载页面 -->
		<form v-on:submit.prevent="onSubmit"></form>

		<!-- 修饰符可以串联 -->
		<a v-on:click.stop.prevent="doThat">

		<!-- 只有修饰符 -->
		<form v-on:submit.prevent></form>

		<!-- 添加事件侦听器时使用 capture 模式 -->
		<div v-on:click.capture="doThis">...</div>

		<!-- 只当事件在该元素本身（而不是子元素）触发时触发回调 -->
		<div v-on:click.self="doThat">...</div>

	```
	- 按键修饰符
	```html
		<!-- 只有在 keyCode 是 13 时调用 vm.submit() -->
		<input v-on:keyup.13="submit">

		<!-- 同上 -->
		<input v-on:keyup.enter="submit">

		<!-- 缩写语法 -->
		<input @keyup.enter="submit">

		// 自定义事件：可以使用 @keyup.f1
		Vue.directive('on').keyCodes.f1 = 112

	```
	- 所有事件是绑定在 `ViewModel` 上，当一个 `ViewModel`被销毁时，所有的事件处理器都会自动被删除
	- 参考 [方法&事件](http://cn.vuejs.org/guide/events.html#u65B9_u6CD5_u5904_u7406_u5668)

6. 表单绑定 -> 参数特性
	- `lazy`
		在默认情况下，`v-model` 在 `input` 事件中同步输入框值与数据，可以添加一个特性 `lazy`，从而改到在 `change` 事件中同步输入框值与数据，可以添加一个特性
		```html
			<!-- 在 "change" 而不是 "input" 事件中更新 -->
			{{msg}} // 只有当按回车时，才会更新值到 DOM 中。
			<input v-model="msg" lazy>
		```
	- `number`
		自动将用户的输入转为 `Number` 类型（如果原值的转换结果为 `NaN` 则返回原值），可以添加一个特性 `number`
		```html
			<input v-model="age" number>
		```
	- `debounce`
		设置一个最小的延时，在每次敲击之后延时同步输入框的值与数据。如果每次更新都要进行高耗操作（例如在输入提示中 `Ajax` 请求），它较为有用
		```html
			<input v-model="age" debounce="500">
		```
		注意 `debounce` 参数不会延迟 `input` 事件：它延迟“写入”底层数据。因此在使用 `debounce` 时应当用 `vm.$watch()` 响应数据的变化。若想延迟 `DOM` 事件，应当使用 `debounce` 过滤器。

7. cameCase vs kebab-case
	- cameCase: **myComponent**
	- kebab-case: **my-component**

8. Component(组件)

	- 组件（Component）是 `Vue.js` 最强大的功能之一。组件可以扩展 `HTML` 元素，封装可重用的代码。在较高层面上，组件是自定义元素，`Vue.js` 的编译器为它添加特殊功能。在有些情况下，组件也可以是原生 `HTML` 元素的形式，以 `is` 特性扩展

	- 当同时存在 `$dispatch()` 和 `v-on` 时，会优先使用 `v-on`，切不会重复发送消息.

    - 父组件模板的内容在父组件作用域内编译；子组件模板的内容在子组件作用域内编译。

    - `Vue.js` 组件 `API` 来自三部分——prop，事件和 slot：

		- prop 允许外部环境传递数据给组件；
		- 事件 允许组件触发外部环境的 `action`；
		- slot 允许外部环境插入内容到组件的视图结构内。

9. 动态添加响应数据属性
	- vm.$set('b', 2);
	- Vue.set(data,'b',2);
	- 推荐在 data 对象上声明所有的响应属性，如果使用上面的方法在初始化后添加，会从新计算 `watch` 值，影响性能。

10. `Vue.js` 默认异步更新 `DOM`.

11. 在 `Vue.js` 中指令和组件分得更清晰。指令只封装 `DOM` 操作，而组件代表一个自给自足的独立单元 —— 有自己的视图和数据逻辑.

12. `Vue.js` 使用基于** 依赖追踪** 的观察系统并且 **异步列队更新**，所有的数据变化都是独立地触发，除非它们之间有明确的依赖关系。唯一需要做的优化是在 v-for 上使用 track-by;`Angular 2` 和 `Vue` 用相似的设计解决了一些 `Angular 1` 中存在的问题;

13. Component 进阶
	- 如果刚看完官方教程对 `component` 理解还不够透彻，或者感觉理解了但是看代码例子时似是而非的感觉。可以看下下面两篇的文章，会对你有帮助的.
		- [Vue.js——60分钟组件快速入门（上篇）](http://www.cnblogs.com/keepfool/p/5625583.html)
		- [Vue.js——60分钟组件快速入门（下篇）](http://www.cnblogs.com/keepfool/p/5637834.html)
	- `Vue.js` 中的组件事件是独立于原生 `DOM` 的事件。
	- Vue.js 组件的 API 来源于三部分——prop，slot 和 事件。
		- prop 允许外部环境传递数据给组件；
		- 事件 允许组件触发外部环境的 action；
		- slot 允许外部环境插入内容到组件的视图结构内

14. vue-router
	`vue-router` 是 `Vue.js` 官方的路由插件，它和 `Vue.js` 是深度集成的，适合用于构建单页面应用。`Vue` 的单页面应用是基于路由和组件的，路由用于设定访问路径，并将路径和组件映射起来。传统的页面应用，是用一些超链接来实现页面切换和跳转的。在 `vue-router` 单页面应用中，则是路径之间的切换，也就是组件的切换.
	- [github 仓库](https://github.com/vuejs/vue-router)
	- [vue-route 2](http://router.vuejs.org/en/index.html) *目前只有英文版本教程*
	- [vue-route 0.7](https://github.com/vuejs/vue-router/tree/1.0/docs/zh-cn) *有中文教程*
	- [vue-router 60 分钟入门](http://www.cnblogs.com/keepfool/p/5690366.html) *以 0.7 版本讲解*

15. vue-resource
	vue-resource 是 Vue.js 的一款插件，它可以通过 XMLHttpRequest 或 JSONP 发起请求并处理响应。也就是说，$.ajax 能做的事情，vue-resource 插件一样也能做到，而且 vue-resource 的 API 更为简洁。另外，vue-resource 还提供了非常有用的 inteceptor 功能，使用 inteceptor 可以在请求前和请求后附加一些行为，比如使用 inteceptor 在 ajax 请求时显示 loading 界面。
	- [vue-resource 全攻略](http://www.cnblogs.com/keepfool/p/5657065.html)





