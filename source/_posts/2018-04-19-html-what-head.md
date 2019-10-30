---
title: 什么是 Head 标签?(二)
date: 2018-04-19 20:07:00
updated: 2018-04-19 20:07:00
categories: Html
---

>在页面加载完成的时候，标签 head 里的内容，是不会在页面中显示出来的。它包含了像页面的 `<title>(标题)` ,CSS(如果你想用 CSS 来美化页面内容)，图标和其他的元数据(比如 作者，关键词，摘要)。在本文中，我们将包含所有上述的事情，为您在脑海中营造一个很好的基础和代码印象。

## 一、什么是 Head 标签?

让我们简单的回顾下上一章提到的 HTML
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My test page</title>
  </head>
  <body>
    <p>This is my page</p>
  </body>
</html>
```

head 标签是 `<head>` 元素的内容。不像 `<body>` 元素的内容可以显示在浏览器中，head 的内容不会在浏览器中显示，它的作用是包含一些页面的元数据。在下面的例子中，head 的内容很少。

```html
<head>
  <meta charset="utf-8">
  <title>My test page</title>
</head>
```

当然，在大型的页面中，head 会包含很多的元数据。你可以用 developer tools 去查看你喜欢的网页的 head 的内容。 在这里，我们会告诉你怎么将必须的内容包含在 head 里，而不是将所有能够包含在 head 里的内容都告诉你。我们开始吧。

## 二、增加一个标题

我们之前已经看到了 `<title>`，它可以用来给 html 文档添加一个标题。你可能会将它和给 body 添加标题的 `<h1>` 元素混淆，有些时候 h1 也会被称作网页标题。但是它们是不同的。

当被加载到浏览器中的时候，元素 `<h1>`  会出现在页面中 —— **通常它应该在一个页面中只被使用一次**, 它被用来标记你的页面内容的标题(故事的标题，新闻标题或者任何适当的方式）
元素 `<title>` 是用来表示 **整个 HTML 文档大致内容的元数据(不是文档的内容)**

### 2.1 交互式学习：检查一个简单的例子
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>&lt;title&gt;element</title>
  </head>
  <body>
    <h1>&lt;h1&gt;element</h1>
  </body>
</html>
```
大家可以把上面的代码拷贝下，保存为 **title-example.html** 文件，然后用浏览器打开，就可以看出下面如有的样子:

![](https://mdn.mozillademos.org/files/12323/title-example.png)

你应该尝试着在你的代码编辑器中打开这些代码，编辑这些元素的内容，然后在你的浏览器中刷新页面。祝你玩得开心。

元素 `<title>` 也被以其他的方式使用着. 比如说，如果你尝试为某个页面添加书签，(Bookmarks > Bookmark This Page, 在火狐浏览器中), 你会看到 `<title>` 的内容被作为建议的书签名.

![](https://mdn.mozillademos.org/files/12337/bookmark-example.png)


## 三、元数据：`<meta>` 元素

元数据就是 **描述数据的数据**，而 HTML 有一个“官方的”方式来为一个文档添加元数据，——  `<meta>` 元素. 当然，其他在这篇文章中提到的东西也可以被当作元数据。 有很多不同种类的 `<meta>` 元素可以被包含进你的页面的 `<head>` 元素, 但是现在我们还不会尝试去解释所有类型, 这只会引起混乱。 我们会解释一些你常会看到的类型，先让你有个概念。

### 3.1 指定你的文档中字符的编码

在上面的例子中，这行是被包含的：

```html
<meta charset="utf-8">
```

这个元素简单的指定了文档的字符编码 —— 在这个文档中被允许使用的字符集。 utf-8 是一个通用的字符集，它包含了任何人类语言中的大部分的字符。 这意味着你的 web 页面可以显示任意的语言; 所以对于你的每一个页面，使用这个设置是很好的! 比如说，你的页面可以很好的处理英语和日语:

![](https://mdn.mozillademos.org/files/12343/correct-encoding.png)



### 3.2 交互式学习： 体验字符集
如果你将你的字符集设置为 ISO-8859-1 (拉丁字母的字符集), 那么你的页面会是乱码的:
为了进行这个练习，回到你在前面 `<title>` 章节中获取的 HTML 模板 ( title-example.html page), 试着改变其字符集的值为 ISO-8859-1, 然后将日语添加到页面中. 这就是我们使用的代码:

```html
<p>Japanese example: ご飯が熱い。</p>
```
![](https://mdn.mozillademos.org/files/12341/bad-encoding.png)


### 3.3 添加作者和描述

许多 `<meta>` 元素包含了name 和 content 特性:

- name 特性指定了 meta 元素的类型; 说明该元素包含了什么类型的信息。
- content 指定了实际的元数据内容。

这两个 meta 元素对于定义你的页面的*作者*和提供页面的*内容描述*是很有用的 。 让我们看一个例子：
```html
<meta name="author" content="Chris Mills">
<meta name="description" content="The MDN Learning Area aims to provide
complete beginners to the Web with all they need to know to get
started with developing web sites and applications.">
```
指定作者在某些情况下是很有用的：如果你需要联系页面的作者，问一些关于页面内容的问题。 一些内容管理系统能够自动获取页面作者的信息，然后用于某种目的。

指定包含关于页面内容的关键字的页面内容的描述是很有用的，因为它可能或让你的页面在搜索引擎的相关的搜索出现得更多 (这些行为术语上被称为 Search Engine Optimization, or SEO.)

### 3.4 实践操作: 在搜索引擎中 description 的使用

description 也被使用在搜索引擎显示的结果页中。 下面通过一个例子来说明

到 front page of The Mozilla Developer Network.
查看页面资源(通过鼠标右键菜单查看页面资源.)
找到 description 的 meta 标签. 就和如下展示的这样:
```html
<meta name="description" content="The Mozilla Developer Network (MDN) provides
information about Open Web technologies including HTML, CSS, and APIs for both
Web sites and HTML5 Apps. It also documents Mozilla products, like Firefox OS.">
```
现在，在你喜欢的搜索引擎里搜索 ”Mozilla Developer Network“ (下图展示的是在雅虎搜索里的情况.) 。你会看到 description `<meta>` and `<title>` 元素如何在搜索结果里显示 — 很值得这样做哦！
A Yahoo search result for "Mozilla Developer Network"

>Note:在谷歌搜索里，在主页面链接下面，你将看到一些相关子页面 — 这些是站点链接,可以在 Google's webmaster tools 配置 — 一种可以使你你的的站点对搜索引擎更友好的方式。

>Note: 许多 `<meta>` 特性已经不再使用. 例如, keyword `<meta>` 元素 — 提供关键词给搜索引擎，根据不同的搜索词，查找到相关的网站 — 被搜索引擎忽略了。 因为作弊者填充了大量关键词到 keyword, 引导搜索结果. 


### 3.5 其他类型的 metadata

当你在网站上查看源码时，你也会发现其他类型的元数据。你在网站上看到的许多功能都是专有创作，旨在向某些网站(如社交网站)提供可使用的特定信息。

例如，Facebook 编写的元数据协议 Open Graph Data 为网站提供了更丰富的元数据。在 MDN 源代码中，你会发现：

```html
<meta property="og:image" content="https://developer.cdn.mozilla.net/static/img/opengraph-logo.dc4e08e2f6af.png">
<meta property="og:description" content="The Mozilla Developer Network (MDN) provides
information about Open Web technologies including HTML, CSS, and APIs for both Web sites
and HTML5 Apps. It also documents Mozilla products, like Firefox OS.">
<meta property="og:title" content="Mozilla Developer Network">
```

上面代码展现的一个效果就是，当你在 Facebook 上链接到 MDN 时，该链接将显示一个图像和描述：这为用户提供更丰富的体验。

![](https://mdn.mozillademos.org/files/12349/facebook-output.png)

Twitter 还拥有自己的类型的专有元数据协议，当网站的 URL 显示在 twitter.com 上时，它具有相似的效果。例如下面：

```html
<meta name="twitter:title" content="Mozilla Developer Network">
```

在你的站点增加自定义图标

为了进一步丰富你的网站设计，你可以在元数据中添加对自定义图标的引用，这些将在特定的场合中显示。

这个不起眼的图标已经存在很多很多年了，16 x 16 像素是这种图标的第一种类型。你可以看见这些图标出现在浏览器每一个打开的页面中的标签页中中以及在书签面板中的书签页面中。


## 四、页面添加图标的方式有：

将其保存在与网站的索引页面相同的目录中，以 **.ico** 格式保存（大多数浏览器将支持更通用的格式，如 **.gif** 或 **.png**，但使用 ICO 格式将确保它能在如 Internet Explorer 6 一样久远的浏览器显示）

将以下行添加到 HTML `<head>` 中以引用它：

```html
<link rel="shortcut icon" href="favicon.ico" type="image/x-icon">
```

现代浏览器在各种场合使用 favicons，如打开的页面标签页和书签面板中的书签页面。下面是一个 favicon 出现在书签面板中的例子：

![](https://mdn.mozillademos.org/files/12351/bookmark-favicon.png)

如今还有很多其他的图标类型可以考虑。 例如，你可以在 MDN 主页的源代码中找到它：

```html
<!-- third-generation iPad with high-resolution Retina display: -->
<link rel="apple-touch-icon-precomposed" sizes="144x144" href="https://developer.cdn.mozilla.net/static/img/favicon144.a6e4162070f4.png">
<!-- iPhone with high-resolution Retina display: -->
<link rel="apple-touch-icon-precomposed" sizes="114x114" href="https://developer.cdn.mozilla.net/static/img/favicon114.0e9fabd44f85.png">
<!-- first- and second-generation iPad: -->
<link rel="apple-touch-icon-precomposed" sizes="72x72" href="https://developer.cdn.mozilla.net/static/img/favicon72.8ff9d87c82a0.png">
<!-- non-Retina iPhone, iPod Touch, and Android 2.1+ devices: -->
<link rel="apple-touch-icon-precomposed" href="https://developer.cdn.mozilla.net/static/img/favicon57.a2490b9a2d76.png">
<!-- basic favicon -->
<link rel="shortcut icon" href="https://developer.cdn.mozilla.net/static/img/favicon32.e02854fdcf73.png">
```

这些注释解释了每个图标的用途 - 这些元素涵盖的东西提供一个高分辨率图标，这些高分辨率图标当网站保存到 iPad 的主屏幕时使用。

不用担心现在实现所有这些类型的图标 - 这是一个相当先进的功能，你将不会被要求在这个课堂上学习这个知识点。 这里的主要目的是让你提前了解有这一样东西以防当你浏览其他网站的源代码时不理解源代码的含义。

## 五、在 HTML 中应用 CSS 和 JavaScript

如今，几乎你使用的所有网站都会使用 CSS 让网页更加炫酷, 使用 JavaScript 让网页有交互功能, 比如视频播放器，地图，游戏以及更多功能。这些应用在网页中很常见，它们分别使用 `<link>` 元素以及 `<script>` 元素。

- `<link>` 元素经常位于文档的头部，它有 2 个属性， `rel="stylesheet`，表明这是文档的样式表，而 href,包含了样式表文件的路径：
```html
<link rel="stylesheet" href="my-css-file.css">
```

- `<script>` 部分没必要非要放在文档头部; 实际上，把它放在文档的尾部（在 `</body>`标签之前）是一个更好的选择 ，这样可以确保在加载脚本之前浏览器已经解析了 HTML 内容（如果脚本加载某个不存在的元素，浏览器会报错）。
```html
<script src="my-js-file.js"></script>
```

注意： `< script >`元素看起来像一个空元素，但它并不是，因此需要一个结束标记。您还可以选择将脚本放入 `< script >`元素中，而不是指向外部脚本文件。

### 5.1 实践操作: 在网页中应用 CSS 和 JavaScript

- 开始操作之前，先拷贝我们的 [meta-example.html](https://github.com/mdn/learning-area/blob/master/html/introduction-to-html/the-html-head/meta-example.html), [script.js](https://github.com/mdn/learning-area/blob/master/html/introduction-to-html/the-html-head/script.js) 和 [style.css](https://github.com/mdn/learning-area/blob/master/html/introduction-to-html/the-html-head/style.css) 文件，并把它们保存到本地电脑的同一目录下，确保使用了正确的文件名和文件格式。

- 使用浏览器和文字编辑器同时打开你的HTML文件。

- 根据上面的信息，添加 `<link>` 和 `<script>` 部分到您的 HTML 文件中, 这样你的 HTML 就可以应用 CSS 和 JavaScript 了。
如果按照上述步骤正确地执行, 当你保存 HTML 文件并重新刷新浏览器的话，你会发现页面已经变样了：

![](https://mdn.mozillademos.org/files/12359/js-and-css.png)

- JavaScript 在页面中添加了一个空列表。现在当你点击列表中的任何地方，浏览器会弹出一个对话框要求你为新列表项输入一些文本内容。 当你点击 OK 按钮，刚刚那个新的列表项会添加到页面上，当你点击那些已有的列表项，会弹出一个对话框允许你修改列表项的文本。

- CSS 使页面背景变成了绿色，文本变得大了一点。它还将 JavaScript 添加到页面中的一些内容进行了样式化，（带有黑色边框的红色条是CSS添加到js生成的列表中的样式。）

>注意：如果你卡在这个练习当中，无法正常应用 CSS 和 JavaScript，试着查看一下我们的  [css-and-js.html](https://github.com/mdn/learning-area/blob/master/html/introduction-to-html/the-html-head/css-and-js.html) 页面实例。


## 六、为文档设定主语言

最后，值得一提的是你可以（而且确实应该）为你的站点设定语言， 这个可以通过添加lang属性到HTML开始标签中来实现 (参考 [meta-example.html](https://github.com/mdn/learning-area/blob/master/html/introduction-to-html/the-html-head/meta-example.html))，如下所示：

```html
<html lang="en-US">
```

这在很多方面都很有用。如果你的 HTML 文档的语言设置好了，那么你的 HTML 文档就会被搜索引擎更有效地索引 (例如，允许它在特定于语言的结果中正确显示),对于那些使用 **屏幕阅读器** 的视障人士也很有用(比如, 法语和英语中都有 “six” 这个单词，但是发音却完全不同)。

你还可以将文档的分段设置为不同的语言。例如， 我们可以把日语部分设置为日语， 如下所示：

```html
<p>Japanese example: <span lang="jp">ご飯が熱い。</span>.</p>
```

这些 codes 根据 ISO 639-1 标准定义的. 你可以在 Language tags in HTML and XML 找到更多相关的。

## 七、总结

已经到了我们快速学习 HTML head 的尾声 — 你还能学到更多的相关的, 但是现阶段详尽的讲的太多会无聊且迷惑, 我们只希望你现在在这学到最基本的概念! 下一篇我们将要学习 HTML text 基础.


## 参考：
- [html head](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/The_head_metadata_in_HTML)
