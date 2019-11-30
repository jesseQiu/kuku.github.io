---
title: 高级文本排版(五)
date: 2018-04-21 10:07:00
updated: 2018-04-21 10:07:00
categories: Html
---

> HTML 中有许多其他元素可以用于格式化文本，我们没有在 [HTML 文字处理基础](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/HTML_text_fundamentals) 中提到它们。本文中所描述的元素虽然少有人知，但仍然值得去学习（尽管仍然不是完整的列表）。在这里你将了解标记引文、描述列表、计算机代码和其他相关文本、下标和上标、联系信息等。
目标: 学习如何使用 *不太知名* 的 HTML 元素来标记高级语义特征。                                                                                                               
## 一、描述列表

在 HTML 基础部分，我们讨论了如何在 HTML 中[标记基本的列表](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/HTML_text_fundamentals#列表_Lists)，但是我们没有提到你偶尔会遇到的第三种类型的列表—**描述列表** (description list) **。**这种列表的目的是 **标记一组项目及其相关描述**，例如 `术语` 和 `定义`，或者是 `问题`和 `答案`等。让我们看一组术语和定义的示例：

```html
soliloquy
In drama, where a character speaks to themselves, representing their inner thoughts or feelings and in the process relaying them to the audience (but not to other characters.)
monologue
In drama, where a character speaks their thoughts out loud to share them with the audience and any other characters present.
aside
In drama, where a character shares a comment only with the audience for humorous or dramatic effect. This is usually a feeling, thought or piece of additional background information
```

描述列表使用与其他列表类型不同的闭合标签— [`<dl>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/dl "HTML <dl> 元素 （或 HTML 描述列表元素）是一个包含术语定义以及描述的列表，通常用于展示词汇表或者元数据 (键-值对列表)。"); 此外，每一项都用  [`<dt>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/dt "HTML <dt> 元素 （或 HTML 术语定义元素）用于在一个定义列表中声明一个术语。该元素仅能作为 <dl> 的子元素出现。通常在该元素后面会跟着 <dd> 元素， 然而，多个连续出现的 <dt> 元素都将由出现在它们后面的第一个 <dd> 元素定义。") (description term)  元素闭合。每个描述都用 [`<dd>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/dd "HTML <dd> 元素（HTML 描述元素）用来指明一个描述列表  (<dl>) 元素中一个术语的描述。这个元素只能作为描述列表元素的子元素出现，并且必须跟着一个 <dt> 元素。") (description description) 元素闭合。让我们来完成下面的标记例子:

```html
<dl>
  <dt>soliloquy</dt>
  <dd>In drama, where a character speaks to themselves, representing their inner thoughts or feelings and in the process relaying them to the audience (but not to other characters.)</dd>
  <dt>monologue</dt>
  <dd>In drama, where a character speaks their thoughts out loud to share them with the audience and any other characters present.</dd>
  <dt>aside</dt>
  <dd>In drama, where a character shares a comment only with the audience for humorous or dramatic effect. This is usually a feeling, thought or piece of additional background information.</dd>
</dl>
```

浏览器的默认样式会在**描述列表的描述部分**（description description）和**描述术语**（description terms）之间产生缩进。MDN非常严密地遵循这一惯例，同时也鼓励关于术语的其他更多的定义（but also embolden the terms for extra definition）。

下面是前述代码的显示结果：

<dl>
    <dt>soliloquy</dt>
    <dd>In drama, where a character speaks to themselves, representing their inner thoughts or feelings and in the process relaying them to the audience (but not to other characters.)</dd>
    <dt>monologue</dt>
    <dd>In drama, where a character speaks their thoughts out loud to share them with the audience and any other characters present.</dd>
    <dt>aside</dt>
    <dd>In drama, where a character shares a comment only with the audience for humorous or dramatic effect. This is usually a feeling, thought or piece of addtional background information.</dd>
</dl>

请注意：一个术语
<font face="consolas, Liberation Mono, courier, monospace"><span style="background-color: rgba(220, 220, 220, 0.5);">&lt;dt&gt;</span></font>可以同时有多个描述 `<dd>`，比如说：

<dl>
    <dt>aside</dt>
    <dd>In drama, where a character shares a comment only with the audience for humorous or dramatic effect. This is usually a feeling, thought or piece of additional background information.</dd>
    <dd>In writing, a section of content that is related to the current topic, but doesn't fit directly into the main flow of content so is presented nearby (often in a box off to the side.)</dd>
</dl>

### 1.1 主动学习: 标记一组定义

现在是时候尝试一下描述列表了; 在输入区域的原始文本里添加相应的元素，使得它在输出区域是以描述列表的形式出现。如果你喜欢，你也可以使用你自己的描述术语和描述。

如果在这其中犯了错误，你可以使用Reset按钮来复原，如果你实在不知道怎么做，你可以按下Show solution按钮看答案。

###### Playable code

```html
<h2>Input</h2>
<textarea id="code" class="input">Bacon
The glue that binds the world together.
Eggs
The glue that binds the cake together.
Coffee
The drink that gets the world running in the morning.
A light brown color.</textarea>
<h2>Output</h2>
<div class="output"></div>
<div class="controls">
  <input id="reset" type="button" value="Reset" />
  <input id="solution" type="button" value="Show solution" />
</div>
```

```css
body {
  font-family: 'Open Sans Light',Helvetica,Arial,sans-serif;
}

.input, .output {
  width: 90%;
  height: 10em;
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
textarea.value = '<dl>\n  <dt>Bacon</dt>\n  <dd>The glue that binds the world together.</dd>\n  <dt>Eggs</dt>\n  <dd>The glue that binds the cake together.</dd>\n  <dt>Coffee</dt>\n  <dd>The drink that gets the world running in the morning.</dd>\n  <dd>A light brown color.</dd>\n</dl>';
  drawOutput();
});

textarea.addEventListener("input", drawOutput);
window.addEventListener("load", drawOutput);
```

<iframe src="https://mdn.mozillademos.org/zh-CN/docs/learn/HTML/Introduction_to_HTML/Advanced_text_formatting$samples/Playable_code?revision=1356002" class="live-sample-frame sample-code-frame" height="500" width="700" id="frame_Playable_code" frameborder="0"></iframe>


## 二、引用

HTML 也有用于标记引用的特性，至于使用哪个元素标记，取决于你引用的是一块还是一行。

### 2.1 块引用

如果一个块级内容（一个段落、多个段落、一个列表等）从其他地方被引用，你应该把它用[`<blockquote>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/blockquote "HTML中的元素（或者 HTML 块级引用元素），代表其中的文字是引用内容。通常在渲染时，这部分的内容会有一定的缩进（注 中说明了如何更改）。若引文来源于网络，则可以将原内容的出处 URL 地址设置到 cite 特性上，若要以文本的形式告知读者引文的出处时，可以通过 <cite> 元素。")元素包裹起来表示，并且在`[cite](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/blockquote#attr-cite)`属性里用URL来指向引用的资源。例如，下面的例子就是引用的MDN的`<blockquote>`元素页面：

```html
<p>The <strong>HTML <code>&lt;blockquote&gt;</code> Element</strong> (or <em>HTML Block
Quotation Element</em>) indicates that the enclosed text is an extended quotation.</p>
```

要把这些转换为块引用，我们要这样做：

```html
<blockquote cite="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/blockquote">
  <p>The <strong>HTML <code>&lt;blockquote&gt;</code> Element</strong> (or <em>HTML Block
  Quotation Element</em>) indicates that the enclosed text is an extended quotation.</p>
</blockquote>
```

浏览器在渲染块引用时默认会增加缩进，作为引用的一个指示符；MDN是这样做的，但是也增加了额外的样式：

> The **HTML `<blockquote>` Element** (or _HTML Block Quotation Element_) indicates that the enclosed text is an extended quotation.

### 2.2 行内引用

行内元素用同样的方式工作，除了使用[`<q>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/q "HTML引用标签 (<q>)表示一个封闭的并且是短的行内引用的文本. 这个标签是用来引用短的文本，所以请不要引入换行符; 对于长的文本的引用请使用 <blockquote> 替代.")元素。例如，下面的标记包含了从MDN`<q>`页面的引用：

```
<p>The quote element — <code>&lt;q&gt;</code> — is <q cite="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/q">intended
for short quotations that don't require paragraph breaks.</q></p>
```

浏览器默认将其作为普通文本放入引号内表示引用，就像下面：

The quote element — `<q>` — is 
<q cite="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/q">intended for short quotations that don't require paragraph breaks.</q>(<q>元素旨在用于不需要分段的短引用)

### 2.3 引文

`[cite](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/blockquote#attr-cite)`属性的内容听起来很有用，但不幸的是，浏览器、屏幕阅读器等等不会真的关心它，如果不使用JavaScript或CSS，浏览器不会显示`cite`的内容。如果你想要确保引用的资源在页面上是可用的，更好的方法是把[`<cite>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/cite "HTML引用（ Citation）标签 (<cite>) 表示一个作品的引用。它必须包含引用作品的符合简写格式的标题或者URL，它可能是一个根据添加引用元数据的约定的简写形式。")元素放到引用元素旁边。这就意味着包含引用资源的名称——即引用的书的名字，或人的名字——但并不表示你不可以用同样的方式把要链接的文本放到`<cite>`元素中：

```
<p>According to the <a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/blockquote">
<cite>MDN blockquote page</cite></a>:
</p>

<blockquote cite="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/blockquote">
  <p>The <strong>HTML <code>&lt;blockquote&gt;</code> Element</strong> (or <em>HTML Block
  Quotation Element</em>) indicates that the enclosed text is an extended quotation.</p>
</blockquote>

<p>The quote element — <code>&lt;q&gt;</code> — is <q cite="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/q">intended
for short quotations that don't require paragraph breaks.</q> -- <a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/q">
<cite>MDN q page</cite></a>.</p>
```

引文默认的字体样式为斜体。你可以在[quotations.html](https://github.com/mdn/learning-area/blob/master/html/introduction-to-html/advanced-text-formatting/quotations.html)中参看代码。

### 2.4 主动学习：是谁说的？

到了主动学习的时间！在这个例子中我们想要你：

1.  把中间的段落变成块引用，它要包含`cite`属性
2.  把第三个段落的一部分变成行内引用，它要包含`cite`属性
3.  每一个引用都要包含`<cite>`元素

可以在线寻找合适的引用资源。

如果你做错了，你总可以使用Reset按钮重置。如果你真的不会了，按下Show solution按钮来看答案。

###### Playable code

```html
<h2>Input</h2>
<textarea id="code" class="input"><p>Hello and welcome to my motivation page. As Confucius once said:</p>

<p>It does not matter how slowly you go as long as you do not stop.</p>

<p>I also love the concept of positive thinking, and The Need To Eliminate Negative Self Talk
(as mentioned in Affirmations for Positive Thinking.)</p></textarea>
<h2>Output</h2>
<div class="output"></div>
<div class="controls">
  <input id="reset" type="button" value="Reset" />
  <input id="solution" type="button" value="Show solution" />
</div>
```

```css
body {
  font-family: 'Open Sans Light',Helvetica,Arial,sans-serif;
}

.input, .output {
  width: 90%;
  height: 10em;
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
textarea.value = '<p>Hello and welcome to my motivation page. As <a href="http://www.brainyquote.com/quotes/authors/c/confucius.html"><cite>Confucius</cite></a> once said:</p>\n\n<blockquote cite="http://www.brainyquote.com/quotes/authors/c/confucius.html">\n  <p>It does not matter how slowly you go as long as you do not stop.</p>\n</blockquote>\n\n<p>I also love the concept of positive thinking, and <q cite="http://www.affirmationsforpositivethinking.com/index.htm">The Need To Eliminate Negative Self Talk</q> (as mentioned in <a href="http://www.affirmationsforpositivethinking.com/index.htm"><cite>Affirmations for Positive Thinking</cite></a>.)</p>';
  drawOutput();
});

textarea.addEventListener("input", drawOutput);
window.addEventListener("load", drawOutput);
```

<iframe src="https://mdn.mozillademos.org/zh-CN/docs/learn/HTML/Introduction_to_HTML/Advanced_text_formatting$samples/Playable_code_2?revision=1356002" class="live-sample-frame sample-code-frame" height="500" width="700" id="frame_Playable_code_2" frameborder="0"></iframe>

## 三、缩略语

另一个你在web上看到的相当常见的元素是[`<abbr>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/abbr "HTML <abbr>元素代表缩写，并可选择提供一个完整的描述。")——它常被用来包裹一个缩略语或缩写，并且提供缩写的解释（包含在`[title](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes#attr-title)`属性中）。让我们看看下面两个例子：

```html
<p>We use <abbr title="Hypertext Markup Language">HTML</abbr> to structure our web documents.</p>

<p>I think <abbr title="Reverend">Rev.</abbr> Green did it in the kitchen with the chainsaw.</p>
```

这些代码的显示效果如下（当光标移动到项目上时会出现提示）：

We use 
<abbr title="Hypertext Markup Language">HTML</abbr> to structure our web documents.

I think 
<abbr title="Reverend">Rev.</abbr> Green did it in the kitchen with the chainsaw.

**Note**: 还有另一个元素<acronym>，它基本上与<abbr>相同，专门用于首字母缩略词而不是缩略语。 然而，这已经被废弃了 - 它在浏览器的支持中不如<abbr>，并且具有类似的功能，所以没有意义。 只需使用<abbr>。

### 3.1 主动学习：标记一个引用

在这个简单的主动学习任务中，我们希望你简单地标记一个缩写。你可以使用下面的示例，或者用自己的示例来替换。

###### Playable code

```html
<h2>Input</h2>
<textarea id="code" class="input"><p>NASA sure does some exciting work.</p></textarea>
<h2>Output</h2>
<div class="output"></div>
<div class="controls">
  <input id="reset" type="button" value="Reset" />
  <input id="solution" type="button" value="Show solution" />
</div>
```

```css
body {
  font-family: 'Open Sans Light',Helvetica,Arial,sans-serif;
}

.input, .output {
  width: 90%;
  height: 5em;
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
textarea.value = '<p><abbr title="National Aeronautics and Space Administration">NASA</abbr> sure does some exciting work.</p>';
  drawOutput();
});

textarea.addEventListener("input", drawOutput);
window.addEventListener("load", drawOutput);
```

<iframe src="https://mdn.mozillademos.org/zh-CN/docs/learn/HTML/Introduction_to_HTML/Advanced_text_formatting$samples/Playable_code_3?revision=1356002" class="live-sample-frame sample-code-frame" height="350" width="700" id="frame_Playable_code_3" frameborder="0"></iframe>


## 四、标记联系方式

HTML有个用于标记联系方式的元素——[`<address>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/address "HTML 的<address>元素可以让作者为它最近的<article>或者<body>祖先元素提供联系信息。在后一种情况下，它应用于整个文档。")。它仅仅包含 **`你`**的联系方式，例如：

```html
<address>
  <p>Chris Mills, Manchester, The Grim North, UK</p>
</address>
```

但要记住的一点是，`<address>`元素是为了标记编写HTML文档的人的联系方式，而不是任何其他的内容。因此，如果这是Chris写的文档，上面的内容将会很好。注意，下面的内容也是可以的：

```html
<address>
  <p>Page written by <a href="../authors/chris-mills/">Chris Mills</a>.</p>
</address>
```

## 五、上标和下标

当你使用日期、化学方程式、和数学方程式时会偶尔使用上标和下标。 [`<sup>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/sup "HTML <sup> 元素定义了一个文本区域，出于排版的原因，与主要的文本相比，应该展示得更高并且更小。") 和[`<sub>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/sub "HTML <sub> 元素定义了一个文本区域，出于排版的原因，与主要的文本相比，应该展示得更低并且更小。")元素可以解决这样的问题。例如：

```html
<p>My birthday is on the 25<sup>th</sup> of May 2001.</p>
<p>Caffeine's chemical formula is C<sub>8</sub>H<sub>10</sub>N<sub>4</sub>O<sub>2</sub>.</p>
<p>If x<sup>2</sup> is 9, x must equal 3 or -3.</p>
```

这些代码输出的结果是：

My birthday is on the 25<sup>th</sup> of May 2001.

Caffeine's chemical formula is C<sub>8</sub>H<sub>10</sub>N<sub>4</sub>O<sub>2</sub>.

If x<sup>2</sup> is 9, x must equal 3 or -3.

## 六、展示计算机代码

有大量的 HTML 元素可以来标记计算机代码：

* [`<code>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/code "HTML <code> 元素呈现一段计算机代码. 默认情况下, 它以浏览器的默认等宽字体显示."): 用于标记计算机通用代码。
* [`<pre>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/pre "HTML <pre> 元素表示预定义格式文本。在该元素中的文本通常按照原文件中的编排，以等宽字体的形式展现出来，文本中的空白符（比如空格和换行符）都会显示出来。(紧跟在 <pre> 开始标签后的换行符也会被省略)"): 用于标记固定宽度的文本块，其中保留空格（通常是代码块）。
* [`<var>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/var "<var> 标签表示变量的名称，或者由用户提供的值。"): 用于标记具体变量名。
* [`<kbd>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/kbd "HTML键盘输入元素(<kbd>) 用于表示用户输入，它将产生一个行内元素，以浏览器的默认monospace字体显示。"): 用于标记输入电脑的键盘（或其他类型）输入。
* [`<samp>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/samp "<samp> 元素用于标识计算机程序输出，通常使用浏览器缺省的 monotype 字体（例如 Lucida Console）。"): 用于标记计算机程序的输出。

让我们看看一些例子。你应该尝试运行一下（尝试运行一下[other-semantics.html](https://github.com/mdn/learning-area/blob/master/html/introduction-to-html/advanced-text-formatting/other-semantics.html)同样的拷贝）：

```html
<pre><code>var para = document.querySelector('p');

para.onclick = function() {
  alert('Owww, stop poking me!');
}</code></pre>

<p>You shouldn't use presentational elements like <code>&lt;font&gt;</code> and <code>&lt;center&gt;</code>.</p>

<p>In the above JavaScript example, <var>para</var> represents a paragraph element.</p>

<p>Select all the text with <kbd>Ctrl</kbd>/<kbd>Cmd</kbd> + <kbd>A</kbd>.</p>

<pre>$ <kbd>ping mozilla.org</kbd>
<samp>PING mozilla.org (63.245.215.20): 56 data bytes
64 bytes from 63.245.215.20: icmp_seq=0 ttl=40 time=158.233 ms</samp></pre>
```

上面的代码显示效果如下：

![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/html-sup-sub.png)

## 七、标记时间和日期

HTML 还支持将时间和日期标记为可供机器识别的格式的 [`<time>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/time "HTML time 标签(<time>) 用来表示24小时制时间或者公历日期，若表示日期则也可包含时间和时区。") 元素。例如：

```html
<time datetime="2016-01-20">20 January 2016</time>
```

为什么需要这样做？因为世界上有许多种书写日期的格式，上边的日期可能被写成：

* 20 January 2016
* 20th January 2016
* Jan 20 2016
* 20/06/16
* 06/20/16
* The 20th of next month
* 20e Janvier 2016
* 2016年1月20日
* And so on

但是这些不同的格式不容易被电脑识别 — 假如你想自动抓取页面上所有事件的日期并将它们插入到日历中，[`<time>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/time "HTML time 标签(<time>) 用来表示24小时制时间或者公历日期，若表示日期则也可包含时间和时区。") 元素允许你附上清晰的、可被机器识别的 时间/日期来实现这种需求。

上述基本的例子仅仅提供了一种简单的可被机器识别的日期格式，这里还有许多其他支持的格式，例如：

```html
<!-- Standard simple date -->
<time datetime="2016-01-20">20 January 2016</time>
<!-- Just year and month -->
<time datetime="2016-01">January 2016</time>
<!-- Just month and day -->
<time datetime="01-20">20 January</time>
<!-- Just time, hours and minutes -->
<time datetime="19:30">19:30</time>
<!-- You can do seconds and milliseconds too! -->
<time datetime="19:30:01.856">19:30:01.856</time>
<!-- Date and time -->
<time datetime="2016-01-20T19:30">7.30pm, 20 January 2016</time>
<!-- Date and time with timezone offset-->
<time datetime="2016-01-20T19:30+01:00">7.30pm, 20 January 2016 is 8.30pm in France</time>
<!-- Calling out a specific week number-->
<time datetime="2016-W04">The fourth week of 2016</time>
```

## 八、总结

到这里你就完成了 HTML 语义文本元素的学习。但要记住，你在本课程中学到的并不是 HTML 文本元素的详细列表 — 我们想要尽量覆盖主要的、通用的、常见的，或者至少是有趣的部分。如果你想找到更多的 HTML 元素，可以看一看我们的[HTML 元素参考](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element)（从 [内联文本语义](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element#内联文本语义)部分开始会是一个好的选择） 。在下一篇文章中我们将会学习用来组织 HTML 文档不同部分的 HTML 元素。



## 参考：
- [高级文本排版](https://developer.mozilla.org/zh-CN/docs/learn/HTML/Introduction_to_HTML/Advanced_text_formatting)
