---
title: Html 文字基础概念(三)
date: 2018-04-19 20:07:00
updated: 2018-04-19 20:07:00
categories: Html
---

>HTML 的主要工作是编辑文本结构和文本内容（也称为语义[semantics](https://developer.mozilla.org/en-US/docs/Glossary/semantics "semantics: In programing, Semantics refers to the meaning of a piece of code — for example "what effect does running that line of JavaScript have?", or "what purpose or role does that HTML element have" (rather than "what does it look like?".)")），以便浏览器能正确的显示。 本文介绍 [HTML](https://developer.mozilla.org/en-US/docs/Glossary/HTML "HTML: HTML (HyperText Markup Language) is a descriptive language that specifies webpage structure.") 的使用方法：在一段文本中添加标题和段落，强调语句，创建列表等等。

>目的: 学习如何用标记(段落、标题、列表、强调、引用)来建立基础文本页面的文本结构和文本内容。                                                                                           

## 一、基础: 标题和段落

大部分的文本结构由标题和段落组成。 不管是小说、报刊、教科书还是杂志等。

![An example of a newspaper front cover, showing use of a top level heading, subheadings and paragraphs.](https://mdn.mozillademos.org/files/12371/newspaper_small.jpg)

内容结构化会使读者的阅读体验更轻松，更愉快。

在 HTML 中，每个段落是通过 [`<p>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/p "HTML <p>元素（或者说 HTML 段落元素）表示文本的一个段落。该元素通常表现为一整块与相邻文本分离的文本，或以垂直的空白隔离或以首行缩进。另外，<p> 是块级元素。") 元素标签进行定义的, 比如下面这样：

```html
<p>I am a paragraph, oh yes I am.</p>
```

每个标题（Heading）是通过“标题标签”进行定义的：

```html
<h1>I am the title of the story.</h1>
```

这里有六个标题元素标签 — [`<h1>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/h1 "此页面仍未被本地化, 期待您的翻译!"), [`<h2>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/h2 "此页面仍未被本地化, 期待您的翻译!"), [`<h3>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/h3 "此页面仍未被本地化, 期待您的翻译!"), [`<h4>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/h4 "此页面仍未被本地化, 期待您的翻译!"), [`<h5>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/h5 "此页面仍未被本地化, 期待您的翻译!"), 和 [`<h6>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/h6 "此页面仍未被本地化, 期待您的翻译!"). 每个元素代表文档中不同级别的内容; `<h1>` 表示主标题（the main heading），`<h2>` 表示二级子标题（subheadings），`<h3>` 表示三级子标题（sub-subheadings），等等。

### 1.1 编辑结构层次

这里举一个例子。在一个故事中，`<h1>` 表示故事的名字，`<h2>` 表示每个章节的标题， `<h3>` 表示每个章节下的子标题，以此类推。

```html
<h1>The Crushing Bore</h1>

<p>By Chris Mills</p>

<h2>Chapter 1: The Dark Night</h2>

<p>It was a dark night. Somewhere, an owl hooted. The rain lashed down on the ...</p>

<h2>Chapter 2: The eternal silence</h2>

<p>Our protagonist could not so much as a whisper out of the shadowy figure ...</p>

<h3>The specter speaks</h3>

<p>Several more hours had passed, when all of a sudden the specter sat bolt upright and exclaimed, "Please have mercy on my soul!"</p>
```

所涉及的元素具体代表什么，完全取决于作者编辑的内容，只要层次结构是合理的。在创建此类结构时，您只需要记住一些最佳实践：

* 优选地，您应该只对每个页面使用一次 `<h1>` — 这是顶级标题，所有其他标题位于层次结构中的下方。
* 请确保在层次结构中以正确的顺序使用标题。不要使用 `<h3>` 来表示副标题，后面跟 `<h2>` 来表示副副标题 - 这是没有意义的，会导致奇怪的结果。
* 在可用的六个标题级别中，您应该旨在每页使用 **不超过三个**，除非您认为有必要使用更多。具有许多级别的文档（即，较深的标题层次结构）变得难以操作并且难以导航。在这种情况下，如果可能，建议将内容分散在多个页面上。

### 1.2 为什么我们需要结构化?

回答这个问题前，让我们先来看一段文档示例 “[text-start.html](https://github.com/mdn/learning-area/blob/master/html/introduction-to-html/html-text-formatting/text-start.html)” — 并从运行这段文档示例（美味的豆沙食谱）开始。首先，您可以复制一份并保存到本地机器上，在之后的练习中您将用到它。在这个文档的主体 （body）中包含了多个内容 — 这些内容没有做任何标记，但是编辑时使用了换行 (输入回车/换行跳转到下一行)处理。

然而，当您在浏览器中打开文档时，您会看到文本显示为一整块！

![A webpage that shows a wall of unformatted text, because there are no elements on the page to structure it.](https://mdn.mozillademos.org/files/12972/text-no-formatting.png)

这是因为没有元素给内容结构，所以浏览器不知道什么是标题，什么是段落。此外：

* 用户在阅读网页时，往往会快速浏览以查找相关内容，经常只是阅读开头的标题（我们通常在一个网页上会花费很少的时间 [spend a very short time on a web page](http://www.nngroup.com/articles/how-long-do-users-stay-on-web-pages/)。如果用户不能在几秒内看到一些有用的内容，他们很可能会感到沮丧并离开。
* 对您的网页建立索引的搜索引擎将标题的内容视为影响网页搜索排名的重要关键字。没有标题，您的网页在 [SEO](https://developer.mozilla.org/en-US/docs/Glossary/SEO "SEO: SEO (Search Engine Optimization) is the process of making a website more visible in search results, also termed improving search rankings.")（搜索引擎优化）方面效果不佳。
* 严重视力障碍者通常不会阅读网页；他们用听力来代替。完成这项工作的软件叫做屏幕阅读器（[screen reader](http://en.wikipedia.org/wiki/Screen_reader "screen readers")）。该软件提供了快速访问给定文本内容的方法。在使用的各种技术中，它们通过朗读标题来提供文档的概述，让用户能快速找到他们需要的信息。如果标题不可用，用户将被迫听到整个文档的大声朗读。
* 使用 [CSS](https://developer.mozilla.org/en-US/docs/Glossary/CSS "CSS: CSS (Cascading Style Sheets) is a declarative language that controls how webpages look in the browser.") 样式化内容，或者使用 [JavaScript](https://developer.mozilla.org/en-US/docs/Glossary/JavaScript "JavaScript: JavaScript (JS) is a programming language mostly used to dynamically script webpages on the client side, but it is also often utilized on the server-side, using packages such as Node.js.") 做一些有趣的事情，你需要包含相关内容的元素，所以 CSS / JavaScript 可以有效地定位它。

因此，我们需要给我们的内容结构标记。

### 1.3 实践操作: 编辑我们的内容结构

让我们直接跳进一个实例。在下面的示例中，向“输入”字段中的原始文本添加元素，使其在“输出”字段中显示为标题和两个段落。

如果您犯了错误，您可以使用 *重置* 按钮进行重置。如果卡住，请按 *显示解决方案* 按钮以查看答案。

###### Playable code

```html
<h2>Input</h2>
<textarea id="code" class="input">My short story

I am a policewoman and my name is Trish.

My legs are made of cardboard and I am married to a fish.</textarea>
<h2>Output</h2>
<div class="output"></div>
<div class="controls">
  <input id="reset" type="button" value="Reset" />
  <input id="solution" type="button" value="Show solution" />
</div>
```


```html
body {
  font-family: 'Open Sans Light',Helvetica,Arial,sans-serif;
}

.input, .output {
  width: 90%;
  height: 8em;
  padding: 10px;
  border: 1px solid #0095dd;
  overflow: auto;
}

button {
  padding: 10px 10px 10px 0;
}
```

```js
var textarea = document.getElementById("code");
var reset = document.getElementById("reset");
var code = textarea.value;
var output = document.querySelector(".output");
var solution = document.getElementById("solution");

function drawOutput() {
  output.innerHTML = textarea.value;
}

reset.addEventListener("click", function() {
  textarea.value = code;
  drawOutput();
});

solution.addEventListener("click", function() {
textarea.value = '<h1>My short story</h1>\n<p>I am a policewoman and my name is Trish.</p>\n<p>My legs are made of cardboard and I am married to a fish.</p>';
  drawOutput();
});

textarea.addEventListener("input", drawOutput);
window.addEventListener("load", drawOutput);
```

<iframe src="https://mdn.mozillademos.org/zh-CN/docs/learn/HTML/Introduction_to_HTML/HTML_text_fundamentals$samples/Playable_code?revision=1345759" class="live-sample-frame sample-code-frame" height="500" width="700" id="frame_Playable_code" frameborder="0"></iframe>

### 1.4 为什么我们需要语义？

在我们身边的任何地方都要依赖语义学 — 我们依靠以前的经验就知道日常事物都代表什么；当我们看到什么，我们就会知道它代表什么。举个例子, 我们知道红色交通灯表示“停止”，绿色交通灯表示”通行“。 如果运用了错误的语义，事情会迅速地变得非常棘手 (难道有某个国家使用红色代表通行？我不希望如此)

同样的道理，我们需要确保使用了正确的元素来给予内容正确的意思、作用以及外形。在这里，[`<h1>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/h1 "此页面仍未被本地化, 期待您的翻译!") 元素也是一个语义元素，它给出了包裹在您的页面上用来表示顶级标题的角色（或意义）的文本。

```html
<h1>这是一个顶级标题</h1>
```

一般来说，浏览器会给它一个更大的字形来让它看上去像个标题（虽然你可以使用 CSS 让它变成任何你想要的样式。更重要的是，它的语义值将以多种方式被使用，比如通过搜索引擎和屏幕阅读器（上文提到过的）。

在另一方面，你可以让任一元素看起来像一个顶级标题，如下：

```html
<span style="font-size: 32px; margin: 21px 0;">Is this a top level heading?</span>
```

这是一个 [`<span>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/span "HTML <span> 元素是短语内容的通用行内容器，并没有任何特殊语义。可以使用它来编组元素以达到某种样式意图（通过使用类或者Id属性），或者这些元素有着共同的属性，比如lang。应该在没有其他合适的语义元素时才使用它。<span> 与 <div> 元素很相似，但 <div> 是一个 块元素 而 <span> 则是  行内元素 .") 元素，它没有语义。当您想要对它用 CSS（或者JS）时，您可以用它包裹内容，且不需要附加任何额外的意义（在未来的课程中你会发现更多这类元素）。我们已经对它使用了 CSS 来让它看起来像一个顶级标题。然而，由于它没有语义值，所以它不会有任何上文提到的帮助。最好的方法是使用相关的 HTML 元素来标记这个项目。


## 二、列表 Lists

现在，让我们注意一下列表。列表在生活中随处可见——从你的购物清单到你的回家路线方案列表，再到你遵从的教程说明列表。在网络上，列表也到处存在，我们需要学习三种不同类型的列表。

### 2.1 无序 Unordered

无序的列表被用来标记每个项目。在这里，项目的顺序并不重要 — 让我们看下面的购物单的例子。

```html
牛奶
鸡蛋
面包
鹰嘴豆泥
```

每份无序的清单从 [`<ul>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/ul "The HTML <ul> 元素 ( 或 HTML 无序列表元素） 代表多项的无序列表，即无数值排序项的集合，且它们在列表中的顺序是没有意义的。通常情况下，无序列表项的头部可以是几种形式，如一个点，一个圆形或方形。头部的风格并不是在页面的HTML描述定义, 但在其相关的CSS 可以用 list-style-type 属性。") 元素开始——这元素包裹了清单上所有被列出的项目：

```html
<ul>
牛奶
鸡蛋
面包
鹰嘴豆泥
</ul>
```

最后一步就是用 [`<li>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/li "HTML <li> 元素 (或者 HTML 列表条目元素) 用于表示列表里的条目。它必须被包含在一个父元素里：一个有顺序的列表(<ol>)，一个无顺序的列表(<ul>)，或者一个菜单 (<menu>)。在菜单或者无顺序的列表里，列表条目通常用点排列显示。在有顺序的列表里，列表条目通常是在左边有按升序排列计数的显示，例如数字或者字母。") 元素把每个列出的项目分别包裹起来：

```html
<ul>
  <li>牛奶</li>
  <li>鸡蛋</li>
  <li>面包</li>
  <li>鹰嘴豆泥</li>
</ul>
```

#### 2.1.1 实践操作: 标记无序列表

尝试编辑下面的样本来创建您个人的 HTML 无序列表。

###### Playable code

```html
<h2>Input</h2>
<textarea id="code" class="input">milk
eggs
bread
humous</textarea>
<h2>Output</h2>
<div class="output"></div>
<div class="controls">
  <input id="reset" type="button" value="Reset" />
  <input id="solution" type="button" value="Show solution" />
</div>
```

```html
body {
  font-family: 'Open Sans Light',Helvetica,Arial,sans-serif;
}

.input, .output {
  width: 90%;
  height: 6em;
  padding: 10px;
  border: 1px solid #0095dd;
  overflow: auto;
}

button {
  padding: 10px 10px 10px 0;
}
```

```js
var textarea = document.getElementById("code");
var reset = document.getElementById("reset");
var code = textarea.value;
var output = document.querySelector(".output");
var solution = document.getElementById("solution");

function drawOutput() {
  output.innerHTML = textarea.value;
}

reset.addEventListener("click", function() {
  textarea.value = code;
  drawOutput();
});

solution.addEventListener("click", function() {
textarea.value = '<ul>\n<li>牛奶</li>\n<li>鸡蛋</li>\n<li>面包</li>\n<li>鹰嘴豆泥</li>\n</ul>';
  drawOutput();
});

textarea.addEventListener("input", drawOutput);
window.addEventListener("load", drawOutput);
```

<iframe src="https://mdn.mozillademos.org/zh-CN/docs/learn/HTML/Introduction_to_HTML/HTML_text_fundamentals$samples/Playable_code_2?revision=1345759" class="live-sample-frame sample-code-frame" height="400" width="700" id="frame_Playable_code_2" frameborder="0"></iframe>


### 2.2 有序 Ordered

有序的列表是根据项目的顺序列出来的——让我们以一组方向为例：

```html
行驶到这条路的尽头
向右转
直行穿过第一个双环形交叉路
在第三个环形交叉路左转
学校就在你的右边，300米处
```

这个标记的结构和无序列表一样，除了你需要用[`<ol>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/ol "HTML <ol> 元素 表示多个有序列表项，通常渲染为有带编号的列表。") 元素将所有项目包裹, 而不是用`<ul>：`

```html
<ol>
  <li>行驶到这条路的尽头</li>
  <li>向右转</li>
  <li>直行穿过第一个双环形交叉路</li>
  <li>在第三个环形交叉路左转</li>
  <li>学校就在你的右边，300米处</li>
</ol>
```

#### 2.2.1 实践操作: 标记有序列表

尝试编辑下面的样本来创建您个人的HTML有序列表：

###### Playable code

```html
<h2>Input</h2>
<textarea id="code" class="input">Drive to the end of the road
Turn right
Go straight across the first two roundabouts
Turn left at the third roundabout
The school is on your right, 300 meters up the road</textarea>
<h2>Output</h2>
<div class="output"></div>
<div class="controls">
  <input id="reset" type="button" value="Reset" />
  <input id="solution" type="button" value="Show solution" />
</div>
```

```html
body {
  font-family: 'Open Sans Light',Helvetica,Arial,sans-serif;
}

.input, .output {
  width: 90%;
  height: 8em;
  padding: 10px;
  border: 1px solid #0095dd;
  overflow: auto;
}

button {
  padding: 10px 10px 10px 0;
}
```

```js
var textarea = document.getElementById("code");
var reset = document.getElementById("reset");
var code = textarea.value;
var output = document.querySelector(".output");
var solution = document.getElementById("solution");

function drawOutput() {
  output.innerHTML = textarea.value;
}

reset.addEventListener("click", function() {
  textarea.value = code;
  drawOutput();
});

solution.addEventListener("click", function() {
textarea.value = '<ol>\n<li>行驶到这条路的尽头</li>\n<li>向右转</li>\n<li>直行穿过第一个双环形交叉路</li>\n<li>在第三个环形交叉路左转</li>\n<li>学校就在你的右边，300米处</li>\n</ol>';
  drawOutput();
});

textarea.addEventListener("input", drawOutput);
window.addEventListener("load", drawOutput);
```

<iframe src="https://mdn.mozillademos.org/zh-CN/docs/learn/HTML/Introduction_to_HTML/HTML_text_fundamentals$samples/Playable_code_3?revision=1345759" class="live-sample-frame sample-code-frame" height="500" width="700" id="frame_Playable_code_3" frameborder="0"></iframe>


### 2.3 实践操作: 标记我们的食谱

到了这里，你拥有了所有你需要的信息来标记我们的食谱样例。你可以选择从[text-start.html](https://github.com/mdn/learning-area/blob/master/html/introduction-to-html/html-text-formatting/text-start.html)复制一份文件并保存在本地，打开它进行编辑，或者在下面的例子中进行编辑。因为在本地你可以保存你的项目，所以在本地做这个工作可能更好。而如果你在下面可编辑的样本中作业，下一次你打开这个网站时你可能会丢失你的数据。各有利弊吧。

###### Playable code

```html
<h2>Input</h2>
<textarea id="code" class="input">Quick humous recipe

    This recipe makes quick, tasty humous, with no messing. It has been adapted from a number of different recipes that I have read over the years.

    Humous is a delicious thick paste used heavily in Greek and Middle Eastern dishes. It is very tasty with salad, grilled meats and pitta breads.

    Ingredients

    1 can (400g) of chick peas (garbanzo beans)
    175g of tahini
    6 sundried tomatoes
    Half a red pepper
    A pinch of cayenne pepper
    1 clove of garlic
    A dash of olive oil

    Instructions

    Remove the skin from the garlic, and chop coarsely
    Remove all the seeds and stalk from the pepper, and chop coarsely
    Add all the ingredients into a food processor
    Process all the ingredients into a paste.
    If you want a coarse "chunky" humous, process it for a short time
    If you want a smooth humous, process it for a longer time

    For a different flavour, you could try blending in a small measure of lemon and coriander, chili pepper, lime and chipotle, harissa and mint, or spinach and feta cheese. Experiment and see what works for you.

    Storage

    Refrigerate the finished humous in a sealed container. You should be able to use it for about a week after you've made it. If it starts to become fizzy, you should definitely discard it.

    Humous is suitable for freezing; you should thaw it and use it within a couple of months.</textarea>
<h2>Output</h2>
<div class="output"></div>
<div class="controls">
  <input id="reset" type="button" value="Reset" />
  <input id="solution" type="button" value="Show solution" />
</div>
```

```html
body {
  font-family: 'Open Sans Light',Helvetica,Arial,sans-serif;
}

.input, .output {
  width: 90%;
  height: 8em;
  padding: 10px;
  border: 1px solid #0095dd;
  overflow: auto;
}

button {
  padding: 10px 10px 10px 0;
}
```

```js
var textarea = document.getElementById("code");
var reset = document.getElementById("reset");
var code = textarea.value;
var output = document.querySelector(".output");
var solution = document.getElementById("solution");

function drawOutput() {
  output.innerHTML = textarea.value;
}

reset.addEventListener("click", function() {
  textarea.value = code;
  drawOutput();
});

solution.addEventListener("click", function() {
  textarea.value = '<h1>Quick hummous recipe</h1>\n\n<p>This recipe makes quick, tasty humous, with no messing. It has been adapted from a number of different recipes that I have read over the years.</p>\n\n<p>Humous is a delicious thick paste used heavily in Greek and Middle Eastern dishes. It is very tasty with salad, grilled meats and pitta breads.</p>\n\n<h2>Ingredients</h2>\n\n<ul>\n<li>1 can (400g) of chick peas (garbanzo beans)</li>\n<li>175g of tahini</li>\n<li>6 sundried tomatoes</li>\n<li>Half a red pepper</li>\n<li>A pinch of cayenne pepper</li>\n<li>1 clove of garlic</li>\n<li>A dash of olive oil</li>\n</ul>\n\n<h2>Instructions</h2>\n\n<ol>\n<li>Remove the skin from the garlic, and chop coarsely.</li>\n<li>Remove all the seeds and stalk from the pepper, and chop coarsely.</li>\n<li>Add all the ingredients into a food processor.</li>\n<li>Process all the ingredients into a paste.</li>\n<li>If you want a coarse "chunky" humous, process it for a short time.</li>\n<li>If you want a smooth humous, process it for a longer time.</li>\n</ol>\n\n<p>For a different flavour, you could try blending in a small measure of lemon and coriander, chili pepper, lime and chipotle, harissa and mint, or spinach and feta cheese. Experiment and see what works for you.</p>\n\n<h2>Storage</h2>\n\n<p>Refrigerate the finished humous in a sealed container. You should be able to use it for about a week after you\'ve made it. If it starts to become fizzy, you should definitely discard it.</p>\n\n<p>Humous is suitable for freezing; you should thaw it and use it within a couple of months.</p>';
  drawOutput();
});

textarea.addEventListener("input", drawOutput);

window.addEventListener("load", drawOutput);
```

<iframe src="https://mdn.mozillademos.org/zh-CN/docs/learn/HTML/Introduction_to_HTML/HTML_text_fundamentals$samples/Playable_code_4?revision=1345759" class="live-sample-frame sample-code-frame" height="500" width="700" id="frame_Playable_code_4" frameborder="0"></iframe>


如果你感到棘手，你可以随时按下_Show solution_按钮，或者在我们的github repo上检查我们的 [text-complete.html](https://github.com/mdn/learning-area/blob/master/html/introduction-to-html/html-text-formatting/text-complete.html) 样例。

### 2.4 嵌套列表 Nesting lists

将一个列表嵌入到另一个列表是完全可以的。 你可能想让一些子项目列在首项目之下。让我们从食谱示例中获取第二个列表：

```html
<ol>
  <li>Remove the skin from the garlic, and chop coarsely.</li>
  <li>Remove all the seeds and stalk from the pepper, and chop coarsely.</li>
  <li>Add all the ingredients into a food processor.</li>
  <li>Process all the ingredients into a paste.</li>
  <li>If you want a coarse "chunky" humous, process it for a short time.</li>
  <li>If you want a smooth humous, process it for a longer time.</li>
</ol>
```

由于最后两项与它们的前一项非常密切相关（它们看起来更像该项的子项或选项），将它们编辑成无序列表，并嵌套在该项的子项中可能更合理。就像下面这样：

```html
<ol>
  <li>Remove the skin from the garlic, and chop coarsely.</li>
  <li>Remove all the seeds and stalk from the pepper, and chop coarsely.</li>
  <li>Add all the ingredients into a food processor.</li>
  <li>Process all the ingredients into a paste.
    <ul>
      <li>If you want a coarse "chunky" humous, process it for a short time.</li>
      <li>If you want a smooth humous, process it for a longer time.</li>
    </ul>
  </li>
</ol>
```

尝试回到上一个实践操作的例子中，并更新第二个列表。

## 三、重点强调

在人类语言中，为了突出一句话的意思，我们通常强调某些词，并且我们通常想要标记某些词作为重点或者在某种程度上的不同。 HTML 提供了许多语义话的元素，并且允许我们通过这些元素的意义标记正文内容，在这个章节中，我们将看到最常见的一小部分元素。

### 3.1 强调

当我们想要在口语中添加强调，我们重读某些词，以便隐含的说出我们想要说的意思。类似的，在写作中，我们通过将文字写成 **斜体** 来强调它。比如，接下来的两个句子就有不同的含义.

I am glad you weren't late.

I am _glad_ you weren't _late_.

第一句话听起来真的像松了一口气因为没有迟到。相反，第二句话听起来具有讽刺性而且有隐含的攻击性，表达对一个人迟到的恼怒。

在 HTML 中我们用 [`<em>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/em "HTML 着重元素 (<em>) 标记出需要用户着重阅读的内容， <em> 元素是可以嵌套的，嵌套层次越深，则其包含的内容被认定为越需要着重阅读。")（emphasis）元素来标记这样的情况。这样做既可以让文档读起来更有趣，也可以被屏幕阅读器识别出来，并以不同的语调发出。浏览器默认风格为斜体，但你不应该纯粹使用这个标签来获得斜体风格，为了获得斜体风格，你应该使用[`<span>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/span "HTML <span> 元素是短语内容的通用行内容器，并没有任何特殊语义。可以使用它来编组元素以达到某种样式意图（通过使用类或者Id属性），或者这些元素有着共同的属性，比如lang。应该在没有其他合适的语义元素时才使用它。<span> 与 <div> 元素很相似，但 <div> 是一个 块元素 而 <span> 则是  行内元素 .")元素和一些CSS，或者是[`<i>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/i "HTML元素 <i> 用于表现因某些原因需要区分普通文本的一系列文本。例如技术术语、外文短语或是小说中人物的思想活动等，它的内容通常以斜体显示。")元素（见下文）。

```
<p>I am <em>glad</em> you weren't <em>late</em>.</p>
```

### 3.2 非常重要

为了强调重要的词，在口语方面我们往往用重音强调，在文字方面则是用粗体字来达到强调的效果。例如下面这段:

This liquid is highly toxic.

I am counting on you. **Do not** be late!

在HTML中我们用[`<strong>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/strong "Strong 元素 (<strong>)表示文本十分重要，一般用粗体显示。") (strong importance) 元素来标记这样的请况。这样做既可以让文档更加地有用，也可以被屏幕阅读器识别出来，并以不同的语调发出。浏览器默认风格为粗体，但你不应该纯粹使用这个标签来获得粗体风格，为了获得粗体风格，你应该使用[`<span>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/span "HTML <span> 元素是短语内容的通用行内容器，并没有任何特殊语义。可以使用它来编组元素以达到某种样式意图（通过使用类或者Id属性），或者这些元素有着共同的属性，比如lang。应该在没有其他合适的语义元素时才使用它。<span> 与 <div> 元素很相似，但 <div> 是一个 块元素 而 <span> 则是  行内元素 .")元素和一些 CSS，或者是 [`<b>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/b "HTML提醒注意（Bring Attention To）元素（<b>）用于吸引读者的注意到该元素的内容上（如果没有另加特别强调）。这个元素过去被认为是粗体（Boldface）元素，并且大多数浏览器仍然将文字显示为粗体。尽管如此，你不应将 <b> 元素用于显示粗体文字；替代方案是使用 CSS font-weight 属性来创建粗体文字。") 元素 (见下文)。

```html
<p>This liquid is <strong>highly toxic</strong>.</p>

<p>I am counting on you. <strong>Do not</strong> be late!</p>
```

如有需要你可以将 strong 元素和 em 元素嵌套在其他的标签中：

```html
<p>This liquid is <strong>highly toxic</strong> —
if you drink it, <strong>you may <em>die</em></strong>.</p>
```

### 3.3 实践操作: 我们是重要的!

在这个实践操作中，我们提供了可编辑的例子。在这个例子中，我们想让你把斜体(em)和加粗(strong)放在你认为重要的词汇上，仅仅为了练习。

###### Playable code

```html
<h2>Input</h2>
<textarea id="code" class="input"><h1>Important notice</h1>

<p>On Sunday January 9th 2010, a gang of goths were
spotted stealing several garden gnomes from a shopping
center in downtown Milwaukee. They were all wearing green
jumpsuits and silly hats, and seemed to be having a whale
of a time. If anyone has any information about this
incident, please contact the police now.</p></textarea>
<h2>Output</h2>
<div class="output"></div>
<div class="controls">
  <input id="reset" type="button" value="Reset" />
  <input id="solution" type="button" value="Show suggestion" />
</div>
```

```html
body {
  font-family: 'Open Sans Light',Helvetica,Arial,sans-serif;
}

.input, .output {
  width: 90%;
  height: 8em;
  padding: 10px;
  border: 1px solid #0095dd;
  overflow: auto;
}

button {
  padding: 10px 10px 10px 0;
}
```

```js
var textarea = document.getElementById("code");
var reset = document.getElementById("reset");
var code = textarea.value;
var output = document.querySelector(".output");
var solution = document.getElementById("solution");

function drawOutput() {
  output.innerHTML = textarea.value;
}

reset.addEventListener("click", function() {
  textarea.value = code;
  drawOutput();
});

solution.addEventListener("click", function() {
  textarea.value = '<h1>Important notice</h1>\n<p>On <strong>Sunday January 9th 2010</strong>, a gang of <em>goths</em> were spotted stealing <strong><em>several</em> garden gnomes</strong> from a shopping center in downtown <strong>Milwaukee</strong>. They were all wearing <em>green jumpsuits</em> and <em>silly hats</em>, and seemed to be having a whale of a time. If anyone has <strong>any</strong> information about this incident, please contact the police <strong>now</strong>.</p>';
  drawOutput();
});

textarea.addEventListener("input", drawOutput);
window.addEventListener("load", drawOutput);
```

<iframe src="https://mdn.mozillademos.org/zh-CN/docs/learn/HTML/Introduction_to_HTML/HTML_text_fundamentals$samples/Playable_code_5?revision=1345759" class="live-sample-frame sample-code-frame" height="500" width="700" id="frame_Playable_code_5" frameborder="0"></iframe>


### 3.4 斜体字、粗体字、下划线...

迄今为止我们已经讨论的元素都是意义清楚的语义元素。[`<b>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/b "HTML提醒注意（Bring Attention To）元素（<b>）用于吸引读者的注意到该元素的内容上（如果没有另加特别强调）。这个元素过去被认为是粗体（Boldface）元素，并且大多数浏览器仍然将文字显示为粗体。尽管如此，你不应将 <b> 元素用于显示粗体文字；替代方案是使用 CSS font-weight 属性来创建粗体文字。"), [`<i>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/i "HTML元素 <i> 用于表现因某些原因需要区分普通文本的一系列文本。例如技术术语、外文短语或是小说中人物的思想活动等，它的内容通常以斜体显示。"), 和 [`<u>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/u "HTML <u> 元素使文本在其内容的基线下的一行呈现下划线。在HTML5中, 此元素表示具有未标注的文本跨度，显示渲染，非文本注释，例如将文本标记为中文文本中的专有名称(一个正确的中文标记), 或 将文本标记为拼写错误。") 的情况却有点复杂。它们出现于人们要在文本中使用粗体、斜体、下划线但 CSS 仍然不被完全支持的时期。像这样的元素，仅仅影响表象而且没有语义，被称为 **表象元素（presentational elements）**并且不应该再被使用。因为正如我们在之前看到的，语义对可访问性，SEO（搜索引擎优化）等非常重要。

HTML5 用新的语义规则重新定义了`<b>`,`<i>`和`<u>`,稍微有点混乱。

这里是最好的经验法则：使用
<font face="consolas, Liberation Mono, courier, monospace"><code>&lt;b&gt;</code>,<code>&lt;i&gt;</code>,<code>&lt;u&gt;</code></font> 来传达传统意义上的粗体，斜体或下划线是合适的，没有其他元素更适合这样用了。然而，总是保持它们拥有可访问性的心态是不对的。斜体的概念对人们使用屏幕阅读器是没有帮助的，对使用其他书写系统而不是拉丁文书写系统的人们也是没有帮助的。

* [`<i>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/i "HTML元素 <i> 用于表现因某些原因需要区分普通文本的一系列文本。例如技术术语、外文短语或是小说中人物的思想活动等，它的内容通常以斜体显示。") 被用来传达传统上用斜体表达的意义：外国文字，分类名称，技术术语，一种思想……
* [`<b>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/b "HTML提醒注意（Bring Attention To）元素（<b>）用于吸引读者的注意到该元素的内容上（如果没有另加特别强调）。这个元素过去被认为是粗体（Boldface）元素，并且大多数浏览器仍然将文字显示为粗体。尽管如此，你不应将 <b> 元素用于显示粗体文字；替代方案是使用 CSS font-weight 属性来创建粗体文字。") 被用来传达传统上用粗体表达的意义：关键字，产品名称，引导句……
* [`<u>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/u "HTML <u> 元素使文本在其内容的基线下的一行呈现下划线。在HTML5中, 此元素表示具有未标注的文本跨度，显示渲染，非文本注释，例如将文本标记为中文文本中的专有名称(一个正确的中文标记), 或 将文本标记为拼写错误。") 被用来传达传统上用下划线表达的意义：专有名词，拼写错误……

一种关于下划线的警告：**人们很容易把下划线和超链接联系起来**。因此，在 Web 上，最好只在链接上使用下划线。当语义适合时使用 `<u>`元素，但是有时候在 Web 上用 CSS 改变下划线默认的的样式更加合适。下面的例子说明了如何做。

```html
<!-- scientific names -->
<p>
  The Ruby-throated Hummingbird (<i>Archilocus colubris</i>)
  is the most common hummingbird in Eastern North America.
</p>

<!-- foreign words -->
<p>
  The menu was a sea of exotic words like <i lang="uk-latn">vatrushka</i>,
  <i lang="id">nasi goreng</i> and <i lang="fr">soupe à l'oignon</i>.
</p>

<!-- a known misspelling -->
<p>
  Someday I'll learn how to <u>spel</u> better.
</p>

<!-- Highlight keywords in a set of instructions -->
<ol>
  <li>
    <b>Slice</b> two pieces of bread off the loaf.
  </li>
  <li>
    <b>Insert</b> a tomato slice and a leaf of
    lettuce between the slices of bread.
  </li>
</ol>
```

## 四、总结

至此，本文应该给您做了一个很好的了解，如何开始在 HTML 中标记文本，并介绍了一些最重要的元素。在这一领域还有许多语义元素，我们将在后面的“更多语义元素”文章中看到更多的语义元素。 在下一篇文章中，我们将详细介绍如何创建超链接（[create hyperlinks](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Creating_hyperlinks)），它可能是 Web 上最重要的元素。

## 参考：
- [HTML text fundamentals](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/HTML_text_fundamentals)
