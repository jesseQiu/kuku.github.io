---
title: Css Viewport
date: 2016-11-30 12:42:57
updated: 2016-11-30 12:42:57
tags:
categories: Css
feature:
---

## 一、什么是 Viewport?

viewport 是用户网页的可视区域。

viewport 翻译为中文可以叫做"视区"。

手机浏览器是把页面放在一个虚拟的"窗口"（viewport）中，通常这个虚拟的"窗口"（viewport）比屏幕宽，这样就不用把每个网页挤到很小的窗口中（这样会破坏没有针对手机浏览器优化的网页的布局），用户可以通过平移和缩放来看网页的不同部分。

## 二、设置 Viewport

一个常用的针对移动网页优化过的页面的 viewport meta 标签大致如下：

* width：控制 viewport 的大小，可以指定的一个值，如果 600，或者特殊的值，如 device-width 为设备的宽度（单位为缩放为 100% 时的 CSS 的像素）。
* height：和 width 相对应，指定高度。
* initial-scale：初始缩放比例，也即是当页面第一次 load 的时候缩放比例。
* maximum-scale：允许用户缩放到的最大比例。
* minimum-scale：允许用户缩放到的最小比例。
* user-scalable：用户是否可以手动缩放。

## 三、测试代码

### 1. 没有添加 viewport
```html
<!DOCTYPE html>
<html>
<body>
<img src="http://img.hb.aicdn.com/34682aaa6c9c1e633cfe8889967bd8d0e92bf9bc12883-XUKHRS_fw658" alt="Chania" width="460" height="345">
<p>
Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim placerat facer possim assum.
</p>
</body>
</html>
```

### 2. 添加 viewport
```html

<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<style>
img {
	max-width:100%;
	height:auto;
}
</style>
</head>
<body>
<img src="http://img.hb.aicdn.com/34682aaa6c9c1e633cfe8889967bd8d0e92bf9bc12883-XUKHRS_fw658" alt="Chania" width="460" height="345">
<p>
Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim placerat facer possim assum.
</p>
</body>
</html>
```

** 注意需要在平板电脑或手机上访问，点击查看效果。**