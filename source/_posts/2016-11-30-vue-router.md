---
title: Vue Router
date: 2016-11-30 09:49:42
updated: 2016-11-30 09:49:42
tags:
categories: Vue.js
feature:
---

> 用 Vue.js + vue-router 创建单页应用，是非常简单的。使用 Vue.js 时，我们就已经把组件组合成一个应用了，当你要把 vue-router 加进来，只需要配置组件和路由映射，然后告诉 vue-router 在哪里渲染它们。

## 一、Getting Started
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue-router</title>
    <script type="text/javascript" src="bower_components/vue/dist/vue.js"></script>
    <!-- <script type="text/javascript" src="node_modules/vuex/dist/vuex.js"></script> -->
    <script type="text/javascript" src="node_modules/vue-router/dist/vue-router.js"></script>
</head>
<body>
<div id="app">
  <h1>Hello App!</h1>
  <p>
    <!-- 使用 router-link 组件来导航. -->
    <!-- 通过传入 `to` 属性指定链接. -->
    <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
    <router-link to="/foo">Go to Foo</router-link> <br>
    <router-link to="/bar">Go to Bar</router-link>
  </p>
  <!-- 路由出口 -->
  <!-- 路由匹配到的组件将渲染在这里 -->
  <router-view></router-view>
</div>
<script type="text/javascript ">
// 0. 如果使用模块化机制编程，導入Vue和VueRouter，要调用 vue.use(vuerouter)
// 1. 定义（路由）组件。
// 可以从其他文件 import 进来
const Foo = { template: '<div>foo</div>' }
const Bar = { template: '<div>bar</div>' }
// 2. 定义路由
// 每个路由应该映射一个组件。 其中"component" 可以是
// 通过 Vue.extend() 创建的组件构造器，
// 或者，只是一个组件配置对象。
// 我们晚点再讨论嵌套路由。
const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar }
]
// 3. 创建 router 实例，然后传 `routes` 配置
// 你还可以传别的配置参数, 不过先这么简单着吧。
const router = new VueRouter({
  routes // （缩写）相当于 routes: routes
})
// 4. 创建和挂载根实例。
// 记得要通过 router 配置参数注入路由，
// 从而让整个应用都有路由功能
const app = new Vue({
  router
}).$mount('#app')
</script>
</body>
</html>
```
> [在线例子](http://jsfiddle.net/yyx990803/xgrjzsup/)

## 二、动态路由匹配
更新上面实例的代码
```html

html:
<router-link to="/jesse/China">Go to Jesse</router-link> <br>
<router-link to="/kuku/USA/school/Harvard">Go to Kuku</router-link>

stript:
<script type="text/javascript ">
const Foo = { template: '<div>foo</div>' }
const Bar = { template: '<div>bar</div>' }
const Jesse = {template: '<div>come from: {{$route.params.country}}</div>'}
const Kuku = {template: '<div>come from: {{$route.params.country}} shchool:{{$route.params.name}}</div>'}

const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar },
  { path: '/jesse/:country', component: Jesse },
  { path: '/kuku/:country/school/:name', component: Kuku }
]

const router = new VueRouter({
  routes // （缩写）相当于 routes: routes
})

const app = new Vue({
  router,
  watch:{
    '$route' (to,from) {
      console.log('to:',JSON.stringify(to),'from:',from.path);
    }
  }
}).$mount('#app')
```

你可以在一个路由中设置多段『路径参数』，对应的值都会设置到 `$route.params` 中。例如：

模式                            | 匹配路径                | $route.params                       
----------------------------- | ------------------- | ------------------------------------
/user/:username               | /user/evan          | `{ username: 'evan' }`              
/user/:username/post/:post_id | /user/evan/post/123 | `{ username: 'evan', post_id: 123 }`

> ** 当使用路由参数时，例如从 /user/foo 导航到 user/bar，原来的组件实例会被复用。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。不过，这也意味着组件的生命周期钩子不会再被调用。 **

> [在线例子](http://jsfiddle.net/yyx990803/4xfa2f19/).

### 高级匹配模式

`vue-router` 使用 [path-to-regexp](https://github.com/pillarjs/path-to-regexp) 作为路径匹配引擎，所以支持很多高级的匹配模式，例如：可选的动态路径参数、匹配零个或多个、一个或多个，甚至是自定义正则匹配。查看它的 [文档](https://github.com/pillarjs/path-to-regexp#parameters) 学习高阶的路径匹配，还有 [这个例子 ](https://github.com/vuejs/vue-router/blob/next/examples/route-matching/app.js) 展示 `vue-router` 怎么使用这类匹配。
