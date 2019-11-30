---
title: HTML 嵌入技术(十一)
date: 2018-05-01 13:08:00
updated: 2018-05-01 13:08:00
categories: Html
---
>到目前为止，您应该掌握了将图像\视频和音频嵌入到网页上的诀窍了。此刻，让我们进行深入学习，来看一些能让您在网页中嵌入各种内容类型的元素：[`<iframe>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe "HTML内联框架元素 <iframe> 表示嵌套的浏览上下文，有效地将另一个HTML页面嵌入到当前页面中。在HTML 4.01中，文档可能包含头部和正文，或头部和框架集，但不能包含正文和框架集。但是，<iframe>可以在正常的文档主体中使用。每个浏览上下文都有自己的会话历史记录和活动文档。包含嵌入内容的浏览上下文称为父浏览上下文。顶级浏览上下文（没有父级）通常是浏览器窗口。"), [`<embed>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/embed "HTML <embed> 元素将外部内容嵌入文档中的指定位置。此内容由外部应用程序或其他交互式内容源（如浏览器插件）提供。") 和[`<object>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/object "HTML <object> 元素（或者称作 HTML 嵌入对象元素）表示引入一个外部资源，这个资源可能是一张图片，一个嵌入的浏览上下文，亦或是一个插件所使用的资源。") 元素。`<iframe>` 用于嵌入其他网页，另外两个元素则允许您嵌入 PDF，SVG，甚至 Flash——一种正在被淘汰的技术，但您仍然会时不时的看到它。

目的:   | 要了解如何使用嵌入物品进入网页[`<object>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/object "HTML <object>元素表示外部资源，可以将其视为图像，嵌套浏览上下文或要由插件处理的资源。")，&nbsp;&nbsp;[`<embed>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/embed "HTML <embed>元素表示外部应用程序或交互式内容（换句话说，插件）的集成点。")以及[`<iframe>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe "HTML <iframe>元素表示嵌套的浏览上下文，有效地将另一个HTML页面嵌入到当前页面中。 在HTML 4.01中，文档可能包含一个头部和一个主体或头部和框架集，但不包括主体和框架集。 但是，一个<iframe>可以在普通文档正文中使用。 每个浏览上下文都有自己的会话历史和活动文档。 包含嵌入内容的浏览上下文称为父浏览上下文。 顶级浏览上下文（没有父级）通常是浏览器窗口。")，像Flash电影和其他网页。

## 一、嵌入的简史

很久以前，在网络上使用 **框架**
创建网站 - 有一小部分存储于个人 HTML 页面的网站是受欢迎的。这些嵌入在一个称为 **框架集**
的主文档中，允许您指定每个框架填充的屏幕上的区域，而不是像表格的列和行的大小。这些被认为是 90 年代中期至 90 年代的凉爽的高度，有证据表明，将网页分解成较小的块，这样更适合下载速度 - 尤其是网络连接如此缓慢。然而，他们有很多问题，远远超过网络速度更快的任何积极因素，所以你看不到它们被使用了。过了一段时间后（20世纪90年代末，21世纪初），插件技术变得非常受欢迎，例如 [Java Applet](https://developer.mozilla.org/en-US/docs/Glossary/Java)和 [Flash](https://developer.mozilla.org/en-US/docs/Glossary/Adobe_Flash)- 这些允许网络开发者将丰富的内容嵌入到视频和动画等网页中，这些网页只能通过 HTML 单独使用。嵌入这些技术是通过诸如 [`<object>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/object "HTML <object>元素表示外部资源，可以将其视为图像，嵌套浏览上下文或要由插件处理的资源。")和较少使用 [`<embed>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/embed "HTML <cke:embed></cke:embed>元素表示外部应用程序或交互式内容（换句话说，插件）的集成点。")的元素来实现的，当时它们非常有用。由于许多问题，包括可访问性、安全性、文件大小等，它们已经过时了;&nbsp;现如今，大多数移动设备不再支持这样的插件，桌面也逐渐不再支持。最后，[`<iframe>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe "HTML <iframe>元素表示嵌套的浏览上下文，有效地将另一个HTML页面嵌入到当前页面中。 在HTML 4.01中，文档可能包含一个头部和一个主体或头部和框架集，但不包括主体和框架集。 但是，一个<iframe>可以在普通文档正文中使用。 每个浏览上下文都有自己的会话历史和活动文档。 包含嵌入内容的浏览上下文称为父浏览上下文。 顶级浏览上下文（没有父级）通常是浏览器窗口。")元素出现了（连同其他嵌入内容的方式，如[`<canvas>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/canvas "使用HTML <canvas>元素与canvas脚本API来绘制图形和动画。")，[`<video>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video "使用HTML <video>元素将视频内容嵌入到文档中。")等），它提供了一种将整个 web 页嵌入到另一个网页的方法，看起来就像那个 web 页是另一个网页的一个[`<img>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img "HTML Image 元素（ <img>）代表文档中的一个图像。")或其他元素一样。[`<iframe>`](https://developer.mozilla.org/zh-CN/ocs/Web/HTML/Element/iframe "HTML内联框架元素 <iframe>表示嵌套的浏览上下文，有效地将另一个HTML页面嵌入到当前页面中。在HTML 4.01中，文档可能包含头部和正文，或头部和框架集，但不能包含正文和框架集。但是，<iframe>可以在正常的文档主体中使用。每个浏览上下文都有自己的会话历史记录和活动文档。包含嵌入内容的浏览上下文称为父浏览上下文。顶级浏览上下文（没有父级）通常是浏览器窗口。")现在经常被使用。了解完历史之后，让我们继续往下看以了解如何使用它们。

## 二、自主学习：嵌入类型的使用

在这篇文章中，我们将直接进入自主学习部分，立即让你体会到嵌入技术的实用性。大家都非常熟悉[Youtube](https://www.youtube.com/)
，但很多人不了解它所提供的一些分享功能。让我们来看看 Youtube 如何让我们通过 [`<iframe>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe "HTML <iframe>元素表示嵌套的浏览上下文，有效地将另一个HTML页面嵌入到当前页面中。 在HTML 4.01中，文档可能包含一个头部和一个主体或头部和框架集，但不包括主体和框架集。 但是，一个<iframe>可以在普通文档正文中使用。 每个浏览上下文都有自己的会话历史和活动文档。 包含嵌入内容的浏览上下文称为父浏览上下文。 顶级浏览上下文（没有父级）通常是浏览器窗口。")
在页面中嵌入喜欢的视频。

1.  首先，去Youtube找一个喜欢的视频。
2.  在视频下方，您会看到一个_共享_按钮 - 点击查看共享选项。
3.  选择“&nbsp;_嵌入”_选项卡，您将得到一些`<iframe>`代码 - 复制一下。
4.  粘贴到下面的_输入_框里，看看_输出_结果是什么。

此外，您还可以试试在示例中嵌入[Google地图](https://www.google.com/maps/)
1.  去Google地图找一个喜欢的地图。
2.  点击UI左上角的“汉堡菜单”（三条水平线）。
3.  选择_共享或嵌入地图_选项。
4.  选择嵌入地图选项，这将给你一些`<iframe>`代码 - 复制一下。
5.  粘贴到下面的_输入_框，看看_输出_结果是什么。

如果你犯了某些错误，你可以点击_Reset按钮以重置编辑器。_如果你确实被卡住了， 按下Show _solution按钮以借鉴答案

###### Playable code

```html
<h2>Input</h2>
<textarea id="code" class="input">
</textarea>
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

.output {
  height: 14em;
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
textarea.value = '<iframe width="420" height="315" src="https://www.youtube.com/embed/QH2-TGUlwu4" frameborder="0" allowfullscreen>\n</iframe>';
  drawOutput();
});

textarea.addEventListener("input", drawOutput);
window.addEventListener("load", drawOutput);
```

<iframe src="https://mdn.mozillademos.org/zh-CN/docs/Learn/HTML/Multimedia_and_embedding/%E5%85%B6%E4%BB%96%E5%B5%8C%E5%85%A5%E6%8A%80%E6%9C%AF$samples/Playable_code?revision=1347732" class="live-sample-frame sample-code-frame" height="600" width="700" id="frame_Playable_code" frameborder="0"></iframe>

<button class="open-in-host button neutral">在 CodePen 中打开 <i class="icon-codepen" aria-hidden="true"></i></button>

<button class="open-in-host button neutral">在 JSFiddle 中打开 <i class="icon-jsfiddle" aria-hidden="true"></i></button>

## 三、Iframe 详解

是不是很简单又有趣呢？[`<iframe>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe "HTML <iframe>元素表示嵌套的浏览上下文，有效地将另一个HTML页面嵌入到当前页面中。 在HTML 4.01中，文档可能包含一个头部和一个主体或头部和框架集，但不包括主体和框架集。 但是，一个<iframe>可以在普通文档正文中使用。 每个浏览上下文都有自己的会话历史和活动文档。 包含嵌入内容的浏览上下文称为父浏览上下文。 顶级浏览上下文（没有父级）通常是浏览器窗口。")
元素旨在允许您将其他Web文档嵌入到当前文档中。这很适合将第三方内容纳入您的网站，您可能无法直接控制，也不希望实现自己的版本 - 例如来自在线视频提供商的视频，[Disqus](https://disqus.com/)
等评论系统，在线地图提供商，广告横幅等。您通过本课程使用的实时可编辑示例使用`<iframe>`实现。我
们会在后面提到，关于`<iframe>`有一些严重的[安全隐患](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Multimedia_and_embedding/%E5%85%B6%E4%BB%96%E5%B5%8C%E5%85%A5%E6%8A%80%E6%9C%AF#安全隐患)需要考虑，但这并不意味着你不应该在你的网站上使用它们 - 它只需要一些知识和仔细的思考。让我们更详细地探索这些代码。假设您想在其中一个网页上加入 MDN 词汇表，您可以尝试以下方式：

```html
<iframe src="https://developer.mozilla.org/en-US/docs/Glossary"
        width="100%" height="500" frameborder="0"
        allowfullscreen sandbox>
  <p> <a href="https://developer.mozilla.org/en-US/docs/Glossary">
    Fallback link for browsers that don't support iframes
  </a> </p>
</iframe>
```

此示例包括使用以下所需的基本要素 `<iframe>`
- `[allowfullscreen](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe#attr-allowfullscreen)` 如果设置，`<iframe>` 则可以使用[全屏API](https://developer.mozilla.org/en-US/docs/Web/Apps/Fundamentals/User_notifications/Full_screen_api)放置在全屏模式（稍微超出本文的范围）。
- `[frameborder](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe#attr-frameborder)` 如果设置为 1，则会告诉浏览器在此框架和其他框架之间绘制边框，这是默认行为。0 删除边框。不推荐这样设置，因为在 `<a href="https://developer.mozilla.org/en-US/docs/Glossary/CSS" title="CSS：CSS（Cascading Style Sheets）是一种声明式语言，用于控制浏览器中网页的外观。">CSS中</a>` 可以更好地实现相同的效果。[`border`](https://developer.mozilla.org/en-US/docs/Web/CSS/border "边框CSS属性是用于一次设置所有单个边框属性值的缩写属性：border-width，border-style和border-color。 与所有速记属性一样，未指定的任何单个值都将设置为其对应的初始值。 重要的是，边框不能用于指定border-image的自定义值，而是将其设置为其初始值，即none。")`: none;`
- `[src](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe#attr-src)`
该属性与[`<video>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video "使用HTML <video>元素将视频内容嵌入到文档中。")
- `[<img>](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img "HTML <img>元素表示文档中的图像。")`一样包含指向要嵌入的文档的URL的路径。
- `[width](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe#attr-width)`和`[height](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe#attr-height)`这些属性指定您想要的iframe的宽度和高度。
- 备选内容与[`<video>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video "使用HTML <video>元素将视频内容嵌入到文档中。")等其他类似元素相同，您可以在打开和关闭`<iframe></iframe>`标签之间包含备选内容，如果浏览器不支持，将会显示 `<iframe>`。在这种情况下，我们已经添加了一个链接到页面。您几乎不可能遇到任何不支持`<iframe>`的浏览器。
- `[sandbox](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe#attr-sandbox)`该属性比其他`<iframe>`功能（例如IE 10及更高版本）稍微更现代的浏览器工作，要求提高安全性设置;我们将在下一节中再说一遍。

**注意**：为了提高速度，在主内容完成加载后，使用 JavaScript 设置 iframe 的 <code>src</code>属性是个好主意。这使您的页面可以更快地使用，并减少您的官方页面加载时间（重要的[SEO](https://developer.mozilla.org/en-US/docs/Glossary/SEO "SEO：SEO（搜索引擎优化）是使网站在搜索结果中更加可见的过程，也称为提高搜索排名。")指标）。

### 3.1 安全隐患

以上我们提到了安全问题 - 现在我们来详细介绍一下这一点。我们并不期望您第一次完全理解所有这些内容;我们只想让您意识到这一问题，并为您提供参考，让您更有经验，并开始考虑 `<iframe>`
在您的实验和工作中使用。此外，没有必要害怕和不使用 `<iframe>` - 你只需要谨慎一点。继续看下去...
浏览器制造商和 Web 开发人员已经学会了如何使用 iframe 作为网络上的坏人（通常被称为<strong>黑客</strong>，或更准确地说是<strong>破解者</strong>）的共同目标（官方术语：**攻击向量**
），如果他们试图恶意修改您的网页或欺骗人们进行不想做的事情，例如显示用户名和密码等敏感信息。因此，规范工程师和浏览器开发人员已经开发了各种安全机制，使其更加安全，并且还有最佳实践要考虑 - 我们将在下面介绍其中的一些。`<iframe>` [单击劫持](https://en.wikipedia.org/wiki/Clickjacking "点击劫持")是一种常见的 iframe 攻击，黑客将隐藏的 iframe 嵌入到您的文档中（或将您的文档嵌入自己的恶意网站），并使用它来捕获用户的交互。这是误导用户或窃取敏感数据的常见方式。一个快速的例子，尽管如此 - 尝试加载在浏览器中上面的例子 - 你可以[在Github上找到它](http://mdn.github.io/learning-area/html/multimedia-and-embedding/other-embedding-technologies/iframe-detail.html)
（[参见源代码](https://github.com/mdn/learning-area/blob/gh-pages/html/multimedia-and-embedding/other-embedding-technologies/iframe-detail.html)
）。你不会看到页面上显示的内容，如果你点击`<a href="https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools">浏览器开发者工具</a>`中的控制台
，你会看到一条消息，告诉你为什么。在 Firefox 中，您会_被 X-Frame-Options 拒绝加载：`https://developer.mozilla.org/en-US/docs/Glossary` 不允许框架化_
。这是因为构建 MDN 的开发人员已经在服务于网站页面的服务器上设置了一个不允许嵌入 `<iframe>`
的设置（请参阅[配置CSP指令](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Multimedia_and_embedding/Other_embedding_technologies#配置CSP指令)
）这是有必要的 - 整个 MDN 页面嵌入在其他页面中并不真实，除非您想要将其嵌入到您的网站上并将其声称为自己的内容，或尝试通过以下方式窃取数据：点击劫持，这两个都是非常糟糕的事情。此外，如果每个人都开始这样做，所有额外的带宽将开始花费 Mozilla 很多资金。

#### 3.1.1 只有在必要时嵌入

有时嵌入第三方内容（例如YouTube视频和地图）是有意义的，但如果您只在完全需要时嵌入第三方内容，您可以省去很多麻烦。网络安全的一个很好的经验法则是_“你怎么谨慎都不为过，如果你决定要做这件事，多检查一遍；如果是别人做的，在被证明是安全的之前，都假设这是危险的。”_除了安全问题，你还应该意识到知识产权问题。无论在线内容还是离线内容，绝大部分内容都是有版权的，甚至是一些你没想到有版权的内容（例如，[Wikimedia Commons](https://commons.wikimedia.org/wiki/Main_Page)上的大多数图片）。不要在网页上展示一些不属于你的内容，除非你是所有者或所有者给了你明确的、书面的许可。对于侵犯版权的惩罚是严厉的。再说一次，你再小心也不为过。如果内容获得许可，你必须遵守许可条款。例如，MDN 上的内容是[在CC-BY-SA下许可的](https://developer.mozilla.org/zh-CN/docs/MDN/About#%E7%89%88%E6%9D%83%E5%92%8C%E8%AE%B8%E5%8F%AF)，这意味着，如果你要引用我们的内容，就必须[用适当的方式注明来源](https://wiki.creativecommons.org/wiki/Best_practices_for_attribution)，即使你对内容做了实质性的修改。

#### 3.1.2 使用 HTTPS

[HTTPS](https://developer.mozilla.org/en-US/docs/Glossary/HTTPS "HTTPS：HTTPS（HTTP Secure）是HTTP协议的加密版本。 它通常使用SSL或TLS来加密客户端和服务器之间的所有通信。 这种安全连接允许客户端安全地与服务器交换敏感数据，例如用于银行活动或在线购物。")是[HTTP](https://developer.mozilla.org/en-US/docs/Glossary/HTTP "HTTP：HTTP（超文本传输​​协议）是启用Web上文件传输的基本协议。 HTTP是文本的（所有的通信都是以纯文本形式进行的）和无状态的（没有通信知道以前的通信）。")的加密版本。您应该尽可能使用 HTTPS 为您的网站提供服务：

1.  HTTPS 减少了远程内容在传输过程中被篡改的机会，
2.  HTTPS 防止嵌入式内容访问您的父文档中的内容，反之亦然。

使用 HTTPS 需要一个安全证书，这可能是昂贵的（尽管[Let's Encrypt](https://letsencrypt.org/)让这件事变得更容易），如果你没有，可以使用 HTTP 来为你的父文档提供服务。但是，由于 HTTPS 的第二个好处，_无论成本如何，您绝对不能使用 HTTP 嵌入第三方内容_（在最好的情况下，您的用户的 Web 浏览器会给他们一个可怕的警告）。所有有声望的公司，例如 Google Maps 或 Youtube，当您嵌入内容时，`<iframe>` 将通过 HTTPS 提供 - 查看 `<iframe>` `src` 属性内的 URL。

**注意**
：[Github页面](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Using_Github_pages)允许默认情况下通过 HTTPS 提供内容，因此对托管内容很有用。如果您正在使用不同的托管，并且不确定，请向您的托管服务商询问。

#### 3.1.3 始终使用 `sandbox`
属性你想给攻击者尽可能少的机会在你的网站上做坏事，那么你应该只给嵌入式内容_工作所需的权限。_
当然，这也适用于你自己的内容。一个代码可以适当使用或用于测试的容器，但不能对其他代码库（意外或恶意）造成任何损害称为[沙箱](https://en.wikipedia.org/wiki/Sandbox_(computer_security))。未沙盒化(Unsandboxed)内容可以做得太多（执行 JavaScript，提交表单，弹出窗口等）默认情况下，您应该使用没有参数的 <code>sandbox</code>属性来强制执行所有可用的限制，如我们前面的示例所示。如果绝对需要，您可以逐个添加权限（`sandbox=""`属性值内） - 请参阅`[sandbox](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe#attr-sandbox)`所有可用选项的参考条目。其中重要的一点是，你_永远不_应该同时添加`allow-scripts`和`allow-same-origin`到你的`sandbox`属性中-在这种情况下，嵌入的内容可以绕过，从执行脚本停止网站同源安全策略，并使用 JavaScript 来关闭完全沙箱。
**注意**：如果攻击者可以直接（外部`iframe`）愚弄人们访问恶意内容，Sandboxing 不提供任何保护。如果某些内容有可能是恶意的（例如，用户生成的内容），请将其从不同的[域服务](https://developer.mozilla.org/en-US/docs/Glossary/domain "域：域是计算机网络的一部分，其中一个实体控制数据处理资源，例如网站。")到您的主要网站。

#### 3.1.4 配置 CSP 指令

[CSP](https://developer.mozilla.org/en-US/docs/Glossary/CSP "CSP：CSP（内容安全策略）用于检测和减轻某些类型的网站相关攻击，如XSS和数据注入。")代表**[内容安全策略](https://developer.mozilla.org/en-US/docs/Web/Security/CSP)**
，它提供[一组HTTP标头](https://developer.mozilla.org/en-US/docs/Web/Security/CSP/CSP_policy_directives)（由web服务器发送时与元数据一起发送的元数据），旨在提高HTML文档的安全性。在`<iframe>`安全性方面，您可以_[将服务器配置为发送适当的 `X-Frame-Options`标题。](https://developer.mozilla.org/en-US/docs/Web/HTTP/X-Frame-Options)这样做可以防止其他网站在其网页中嵌入您的内容（这将导致[点击](https://en.wikipedia.org/wiki/clickjacking "点击劫持")和一系列其他攻击），正如我们之前看到的那样，MDN 开发人员已经做了这些工作。

**注意**
：您可以阅读 Frederik Braun 的帖子
`<a href="https://blog.mozilla.org/security/2013/12/12/on-the-x-frame-options-security-header/" class="external external-icon" rel="noopener">在X-Frame-Options安全性头上</a>`获取有关此主题的更多背景信息。显然，在这篇文章中已经解释得很清楚了。

## 四、&lt;embed&gt; 和 &lt;object&gt; 元素
[embed](https:/`/developer.mozilla.org/en-US/docs/Web/HTML/Element/embed ) 和 [object](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/object ) 元素的功能不同于[iframe](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe )——这些元素是用来嵌入多种类型的外部内容的通用嵌入工具，其中包括像 Java 小程序和 Flash，PDF（可在浏览器中显示为一个 PDF 插件）这样的插件技术，甚至像视频，SVG 和图像的内容！

**注意**：**插件**是一种对浏览器原生无法读取的内容提供访问权限的软件。然而，您不太可能使用这些元素 - Applet 几年来一直没有被使用；由于许多原因，Flash 不再受欢迎（见下面的[插件案例](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Multimedia_and_embedding/Other_embedding_technologies#The_case_against_plugins)）；PDF 更倾向于被链接而不是被嵌入；其他内容，如图像和视频都有更优秀、更容易元素来处理。插件和这些嵌入方法真的是一种传统技术，我们提及它们主要是为了以防您在某些情况下遇到问题，比如内部网或企业项目等。如果您发现自己需要嵌入插件内容，那么您至少需要一些这样的信息：

![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/embed-object.png)

**注意**：<`object>``需要`data`属性，`type`属性或两者。如果您同时使用这两个，您也可以使用该`[typemustmatch](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/object#attr-typemustmatch)`属性（仅在Firefox中实现，在本文中）。`typemustmatch`保持嵌入文件不运行，除非`type`属性提供正确的媒体类型。`typemustmatch`因此，当您嵌入来自不同[来源的](https://developer.mozilla.org/en-US/docs/Glossary/origin "来源：Web内容的起源由方案（协议），主机（域）和用于访问它的URL的端口定义。 只有当方案，主机和端口都匹配时，两个对象具有相同的原点。")内容（可以防止攻击者通过插件运行任意脚本）时，可以赋予重要的安全优势。

下面是一个使用该[`<embed>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/embed "HTML <embed>元素表示外部应用程序或交互式内容（换句话说，插件）的集成点。")元素嵌入 Flash 影片的示例（请参阅此处的[Github](http://mdn.github.io/learning-area/html/multimedia-and-embedding/other-embedding-technologies/embed-flash.html)，并[检查源代码](https://github.com/mdn/learning-area/blob/gh-pages/html/multimedia-and-embedding/other-embedding-technologies/embed-flash.html)）：

```html
<embed src="whoosh.swf" quality="medium"
       bgcolor="#ffffff" width="550" height="400"
       name="whoosh" align="middle" allowScriptAccess="sameDomain"
       allowFullScreen="false" type="application/x-shockwave-flash"
       pluginspage="http://www.macromedia.com/go/getflashplayer">
```

很可怕，不是吗 。Adobe Flash 工具生成的 HTML 往往更糟糕，使用嵌入 `<object>` 元素的 `<embed>`
元素来覆盖所有的基础（查看一个例子）。甚至有一段时间，Flash 被成功地用作 HTML5 视频的备用内容，但是这种情况越来越被认为是不必要的。现在来看一个 `<object>`
将 PDF 嵌入一个页面的例子（参见 `<a href="http://mdn.github.io/learning-area/html/multimedia-and-embedding/other-embedding-technologies/object-pdf.html" class="external external-icon" rel="noopener">实例</a>`和[源代码](https://github.com/mdn/learning-area/blob/gh-pages/html/multimedia-and-embedding/other-embedding-technologies/object-pdf.html)
）：

```html
<object data="mypdf.pdf" type="application/pdf"
        width="800" height="1200" typemustmatch>
  <p>You don't have a PDF plugin, but you can <a href="myfile.pdf">download the PDF file.</a></p>
</object>
```

PDF 是纸与数据之间重要的阶梯，但它们[在可访问性上有些问题](http://webaim.org/techniques/acrobat/acrobat)
`<a href="http://webaim.org/techniques/acrobat/acrobat" class="external external-icon" rel="noopener">，</a>`并且可能难以在小屏幕上阅读。它们在一些圈子中仍然受欢迎，我们最好是用链接指向它们，而不是将其嵌入到网页中，以便它们可以在单独的页面上被下载或被阅读。

### 4.1 针对插件的情况

以前，插件在网络上是不可或缺的。还记得你必须安装 Adobe Flash Player 才能在线观看电影的日子吗？并且你还会不断地收到关于更新 Flash Player 和 Java 运行环境的烦人警报。Web 技术已经变得更加强大，那些日子已经结束了。对于大多数应用程序，现在是停止依赖插件传播内容，开始利用 Web 技术的时候了。

* **扩大你对大家的影响力。**

    每个人都有一个浏览器，但插件越来越少，特别是在移动用户中。由于 Web 在很大程度上不需要依赖插件而运行，所以人们宁愿只是去竞争对手的网站而不是安装插件。

* **从 Flash 和其他插件附带的[额外辅助功能头痛](http://webaim.org/techniques/flash/)
中脱颖而出。**

* **避免额外的安全隐患。**

    即使经过无数次补丁[，](http://www.cvedetails.com/product/6761/Adobe-Flash-Player.html?vendor_id=53)&nbsp;Adobe Flash也是<a href="http://www.cvedetails.com/product/6761/Adobe-Flash-Player.html?vendor_id=53" class="external external-icon" rel="noopener">非常不安全的</a>。2015年，Facebook的首席安全官Alex Stamos甚至[要求Adobe停止Flash。](http://www.theverge.com/2015/7/13/8948459/adobe-flash-insecure-says-facebook-cso)

那你该怎么办？如果您需要交互性，HTML 和 [JavaScript](https://developer.mozilla.org/en-US/docs/Glossary/JavaScript "JavaScript：JavaScript（JS）是一种编程语言，主要用于客户端来动态地脚本化网页，但也常常是服务器端的。")可以轻松地为您完成工作，而不需要 Java 小程序或过时的 ActiveX / BHO 技术。您可以使用[HTML5视频](https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Add_audio_or_video_content_to_a_webpage)来满足媒体需求，矢量图形[SVG](https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Add_vector_image_to_a_webpage)
，以及复杂图像和动画[画布](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial)。[彼得·埃尔斯特（Peter Elst）几年前已经在写](https://plus.google.com/+PeterElst/posts/P5t4pFhptvp)，对于工作Adobe Flash 极少是正确的工具，除了专门的游戏和商业应用。对于 ActiveX，即使微软的[Edge](https://developer.mozilla.org/en-US/docs/Glossary/Microsoft_Edge "Edge：Microsoft Edge是一种免费的图形Web浏览器，与Microsoft Windows捆绑在一起，由Microsoft自2014年开始。最初称为Spartan，Edge取代了长期以来的Microsoft浏览器Internet Explorer。")
浏览器也不再支持。

## 五、概要

在 Web 文档中嵌入其他内容的主题可能会变得非常复杂，因此在本文中，我们尝试以一种简单而熟悉的方式来介绍它，它们有直接的关系，同时仍然暗示了一些涉及更高级功能的技术。首先，您不可能在包含第三方内容（如地图和视频）的网页上使用嵌入式广告。当你变得更有经验的时候，你可能会开始为他们找到更多的用途。除了我们在这里讨论的那些外，还有许多涉及嵌入外部内容的技术。我们看到了一些在前面的文章中出现的，如[`<video>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video "使用HTML <video>元素将视频内容嵌入到文档中。")
，[`<audio>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/audio "HTML <audio>元素用于在文档中嵌入声音内容。 它可能包含一个或多个音频源，使用src属性或<source>元素表示：浏览器将选择最合适的一个。 它也可以是流媒体的目的地，使用MediaStream。")
和[`<img>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img "HTML <img>元素表示文档中的图像。")，但也有其它的有待关注，如&nbsp;&nbsp;[`<canvas>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/canvas "使用HTML <canvas>元素与canvas脚本API来绘制图形和动画。")用于 JavaScript 生成的 2D 和 3D 图形，`[<svg>](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/svg "关于此的文件尚未写入;  请考虑贡献！")`用于嵌入矢量图形。

## 参考：
- [
从对象到iframe - 其他嵌入技术](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Multimedia_and_embedding/%E5%85%B6%E4%BB%96%E5%B5%8C%E5%85%A5%E6%8A%80%E6%9C%AF)