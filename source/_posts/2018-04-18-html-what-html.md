---
title: 什么是 HTML?(一)
date: 2018-04-18 11:07:00
updated: 2018-04-18 11:07:00
categories: Html
---

>超文本标记语言  (英语：**H**yper**t**ext **M**arkup **L**anguage，简称：HTML)  是一种用来结构化 Web 网页及其内容的标记语言。例如，你的内容可以是一组段落或一个重点列表，甚至可以含有图片和数据表。这篇文章提供的内容，能使你对 HTML 语言及其功能有最基本的认识。

## 一、什么是 HTML？

HTML 并不是真正的编程语言，它是一种用于定义内容结构的_标记语言_它的复杂程度可由网站开发者决定。HTML 由一系列的**元素（[elements](https://developer.mozilla.org/en-US/docs/Glossary/element "elements: An element is a part of a webpage. In XML and HTML, an element may contain a data item or a chunk of text or an image, or perhaps nothing. A typical element includes an opening tag with some attributes, enclosed text content, and a closing tag.
")）**所组成，这些元素可以用来封装不同部分的内容，使其以某种方式呈现或者工作。 闭合标签（ [tags](https://developer.mozilla.org/en-US/docs/Glossary/tag "tags: In HTML a tag is used for creating an element.  The name of an HTML element is the name used in angle brackets such as <p> for paragraph.  Note that the end tag's name is preceded by a slash character, "</p>", and that in empty elements the end tag is neither required nor allowed. If attributes are not mentioned, default values are used in each case.")）可以使得一个文字或者一张图片连接到其他网址，可以使得文字为斜体，可让文字变大或者变小等等。 例如，键入下面一行内容：

```html
My cat is very grumpy
```

如果我们想要比较正式的去指明这是一个段落，我们可以将其封装成为一个段落 paragraph 元素：([`<p>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/p "HTML <p>元素（或者说 HTML 段落元素）表示文本的一个段落。该元素通常表现为一整块与相邻文本分离的文本，或以垂直的空白隔离或以首行缩进。另外，<p> 是块级元素。")) ：

```html
<p>My cat is very grumpy</p>
```

### 1.1 解析一个HTML元素

让我们深入探索一下这个段落paragraph元素。

![](https://mdn.mozillademos.org/files/9347/grumpy-cat-small.png)

这个元素的主要部分有：

1.  开始标签（The opening tag）：这里包含了元素的名称（本例为 p），被开、闭**尖括号**所包围。这表示元素从此开始或者开始起作用——在本例中即段落由此开始。
2.  闭合标签（The closing tag）：与开始标签相似，只是其在元素名之前包含了一个斜杠。这表示着元素的结尾——在本例中，就是这一段落的结尾。
3.  内容（The content）：这是一个元素的内容，这个例子中就是所输入的文本本身。
4.  元素（The element）：开标签、闭标签与内容相结合，便是一个完整的元素。

元素也可以有属性，看起来像这样：

![](https://mdn.mozillademos.org/files/9345/grumpy-cat-attribute-small.png)

属性（Attribute）包含了关于元素的一些额外的信息，这些信息本身并不需要被显现在内容（Content）中。在这个例子中，`class` 是一个属性名称（name），`editor-note` 是属性的值(value) 。`class` 属性允许你为元素提供一个标识名称，以便进一步为元素指定样式或进行其他操作时使用。

一个属性应该包含：

1.  在属性与元素名称或上一个属性（如果有超过一个属性的话）之间的空格符
2.  属性的名称，并接上一个等号
3.  由引号所包围的属性值

### 1.2 嵌套元素

你也可以将一个元素置于其他元素之中——这被称作嵌套。如果你想表明你的猫非常非常的暴躁，我们可以将 “非常” 用 [`<strong>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/strong "Strong 元素 (<strong>)表示文本十分重要，一般用粗体显示。") 元素包裹，这意味着这个单词将被强调：

```html
<p>My cat is <strong>very</strong> grumpy.</p>
```

你必须保证你的元素被正确地嵌套：在这个例子中我们首先使用 p 标签，然后是 strong 标签，接着我们需要先闭合 strong 标签，最后再闭合 p 标签。下面是一个错误示范：

```html
<p>My cat is <strong>very grumpy.</p></strong>
```

元素必须正确地开始和闭合，才能清楚地显示出正确的嵌套。如果它们像上面这样，那么你的浏览器将尽量地猜测你想要表达的意思，但是你很大程度上不会得到你期望的结果。所以千万不要这样做！

### 1.3 空元素

有一些元素并不包含内容，它们被称为空元素。看看我们 HTML 代码中的 [`<img>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img "HTML Image 元素（ <img> ）代表文档中的一个图像。") 元素：

```html
<img src="images/firefox-icon.png" alt="My test image">
```

它包含了两个属性，但是这里并没有 `</img>`闭合标签，也没有内部内容。因为 `image`元素不包含内容是有效果的，它的作用是向其所在的位置嵌入一个图像。


### 1.4 解析HTML文档

上面我们介绍了一些基本的 HTML 元素，但是他们自己不是很有用。现在我们看看如何将它们组合起来成为一个完整的 HTML 页面。让我们重新看看  `index.html` 示例里的内容（我们先前在 [文件处理](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/Dealing_with_files) 里创建的）：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My test page</title>
  </head>
  <body>
    <img src="images/firefox-icon.png" alt="My test image">
  </body>
</html>
```

这里我们有：

* `<!DOCTYPE html>` — 文档类型（doctypes）。在 HTML 刚刚出现的时期里（大约是1991/2 年），文档类型是用来链接一些应该遵守的规则，有点像自动校正等。然而现在大家都不用管文件类型，只是因为历史原因必须包含在代码中。现阶段大家知道这些就够了。
* `<html></html>` — `<html>` 元素. 这个元素包含了整个页面的内容，有时也被称作根元素。
* `<head></head>` — `<head>` 元素. 这个元素可以包含你想添加的所有内容，但是不会被用户所看到。这里面包括像想被搜索引擎搜索到的关键字（[keywords](https://developer.mozilla.org/en-US/docs/Glossary/keyword "keywords: A keyword is a word or phrase that describes content.  Online keywords are used as queries for search engines or as words identifying content on websites.")）和页面描述，CSS样式表和字符编码声明等等。
* `<body></body>` — `<body>` 元素. 这个元素包含了你想被用户看到的内容，不管是文本，图像，视频，游戏，可播放的音轨或是其他内容。
* `<meta charset="utf-8">` — 这个元素指定了你的文档需要使用的字符编码，像 UTF-8 ，它包括了非常多人类已知语言的字符。基本上 UTF-8 可以处理任何文本内容。我们没有任何理由不去设定字符编码，而且也可以避免以后可能出现的问题。
* `<title></title>` — 这个元素设置了页面的标题，标题显示在浏览器标签页上，而且在你将网页添加到收藏夹或喜爱中时将作为默认名称。

## 二、图像

让我们重新回到 image 元素：

```html
<img src="images/firefox-icon.png" alt="My test image">
```

像我们之前说过的，它将图像嵌入到元素所在位置。它通过包含图像文件路径的  `src` (source) 属性实现这一功能。

但是这一元素也要包括 `alt` (alternative) 属性 —— 这个属性应该是图像的描述内容，当图像不能被某些用户看见时，可能是因为：

1.  他们是盲人或者有视觉障碍。某些有严重视觉损伤的用户经常使用屏幕阅读器来为他们读出 `alt `属性里的内容。
2.  有些代码里的错误让图像不能被展示出来。举个例子，故意将 `src` 属性里的路径改错。如果你保存并且重新加载页面，你应该能在图像的位置看到：

![](https://mdn.mozillademos.org/files/9349/alt-text-example.png)

alt 属性的关键就是要“可以描述图像的文本”。当被读出来时， alt 里面的内容应该向用户传递足够图像表达的意思。所以我们现在的文本 "My test image" 并不合适。一个描述火狐Logo的更好的文本应该是 "The Firefox logo: a flaming fox surrounding the Earth."。

**现在为你的图像尝试一些更好的 alt 文本。**

提示： __点击 [MDN's Accessibility landing page](https://developer.mozilla.org/en-US/docs/Web/Accessibility) _查看更多_。


## 三、标记文本
这一部分包含了一些基本的标记文本的 HTML 元素。

### 3.1 标题

标题元素允许你指定内容的标题和子标题。就像一本书拥有主标题，然后每一章还有标题，然后还有更小的标题，HTML 文档也是一样。HTML 包括六个级别的标题， [`<h1>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/h1 "此页面仍未被本地化, 期待您的翻译!")–[`<h6>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/h6 "此页面仍未被本地化, 期待您的翻译!") ，虽然你可能只会用到 3-4 最多。

```html
<h1>My main title</h1>
<h2>My top level heading</h2>
<h3>My subheading</h3>
<h4>My sub-subheading</h4>
```

现在尝试一下，在你的 [`<img>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img "HTML Image 元素（ <img> ）代表文档中的一个图像。") 元素上面添加一个合适的标题。

### 3.2 段落

像上面解释过的一样，[`<p>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/p "HTML <p>元素（或者说 HTML 段落元素）表示文本的一个段落。该元素通常表现为一整块与相邻文本分离的文本，或以垂直的空白隔离或以首行缩进。另外，<p> 是块级元素。") 元素是用来指定段落的。你可以用它来指定常规的文本内容：

```html
<p>This is a single paragraph</p>
```

**添加你的样本内容（从** **_[What should your website look like?](https://developer.mozilla.org/en-US/Learn/Getting_started_with_the_web/What_should_your_web_site_be_like) 获得_****）到一个或几个段落里，然后将它们放在 `<img>` 元素之下。**

### 3.3 列表

很多Web上的内容都是列表，所以 HTML 有一些特别的列表元素。要标记列表通常包括至少两个元素。最常用的列表类型是有序列表（ ordered lists）和无序列表（ unordered lists）：

1.  **无序列表** 中项目的顺序并不重要，就像购物列表。这些内容被包括在一个 [`<ul>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/ul "The HTML <ul> 元素 ( 或 HTML 无序列表元素） 代表多项的无序列表，即无数值排序项的集合，且它们在列表中的顺序是没有意义的。通常情况下，无序列表项的头部可以是几种形式，如一个点，一个圆形或方形。头部的风格并不是在页面的HTML描述定义, 但在其相关的CSS 可以用 list-style-type 属性。") 元素里。
2.  **有序列表** 中项目的顺序很重要，就像一个食谱。这些内容被包括在一个 [`<ol>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/ol "HTML <ol> 元素 表示多个有序列表项，通常渲染为有带编号的列表。") 元素里。

列表内的每个项目被包括在一个 [`<li>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/li "HTML <li> 元素 (或者 HTML 列表条目元素) 用于表示列表里的条目。它必须被包含在一个父元素里：一个有顺序的列表(<ol>)，一个无顺序的列表(<ul>)，或者一个菜单 (<menu>)。在菜单或者无顺序的列表里，列表条目通常用点排列显示。在有顺序的列表里，列表条目通常是在左边有按升序排列计数的显示，例如数字或者字母。") (list item)元素里。

所以，如果我们想要将下面的段落碎片改成一个列表：

```html
<p>At Mozilla, we’re a global community of technologists, thinkers, and builders working together ... </p>
```

我们可以这样做：

```html
<p>At Mozilla, we’re a global community of</p>

<ul> 
  <li>technologists</li>
  <li>thinkers</li>
  <li>builders</li>
</ul>

<p>working together ... </p>
```

**尝试添加一个有序列表和无序列表到你的示例页面。**

## 四、链接

链接非常重要 — 它们让 Web 成为了 WEB（万维网）。要植入一个链接，我们需要使用一个简单的link — [`<a>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a "HTML <a> 元素  (或锚元素) 可以创建一个到其他网页、文件、同一页面内的位置、电子邮件地址或任何其他URL的超链接。") — a 是 "anchor" （锚）的缩写。要将一些文本添加到链接中，只需如下几步：

1.  添加一些文本。我们选择“Mozilla Manifesto”。
2.  将文本包含在 `<a>` 元素内，就像这样：
```html
<a>Mozilla Manifesto</a>
```

3.  赋予 `<a>` 元素一个 href 属性，就像这样：
```html
<a href="">Mozilla Manifesto</a>
```

4.  把你想要链接的网址放到 href 属性内：
```html
<a href="http://blog.luckykuku.com">Mozilla Manifesto</a>
```

如果你在网址开始部分省略了 `https://` 或者 `http://` 协议，你可能会得到错误的结果。在完成一个链接后，点击它并确保它去向了你指定的网址。

## 参考：
- [html basics](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/HTML_basics)
