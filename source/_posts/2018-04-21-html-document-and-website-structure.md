---
title: 文件和网站结构(六)
date: 2018-04-21 20:07:00
updated: 2018-04-21 20:07:00
categories: Html
---
>除了定义网页的各个部分（例如“段落”或“图片”）外，[HTML](https://developer.mozilla.org/en-US/docs/Glossary/HTML "HTML: HTML (HyperText Markup Language) is a descriptive language that specifies webpage structure.") 还拥有一些用于定义网站区域的块级元素(例如“头部”，“导航菜单”，“主要内容列”)。本文将探讨如何规划基本的网站结构，并通过编写HTML来表示这种网站结构。
目标: | 了解如何使用语义标签来构建文档，以及如何制定简单网站的结构

## 一、文档的基本部分

网页可以看起来彼此不同，但它们都倾向于使用类似的标准组件，除非页面显示全屏视频或游戏，或是某种艺术项目的一部分，或者是结构不当：

<dl>
    <dt>标题</dt>
    <dd>通常在顶部有一个大标题和（或）图标。 这是一个网站的主要常见信息，通常存在于每一个网页。</dd>
    <dt>导航</dt>
    <dd>链接到网站的主要部分；通常由菜单按钮、链接或选项卡表示。与标题一样，这些内容通常在一个网页与另一个网页之间保持一致——在您的网站上导航不一致只会使人疑惑和恼火。许多网页设计师认为导航栏是标题的一部分，而不是独立的组件，但这并不是一个硬性规定；实际上有些人认为，两个独立的会有更好的 [可访问性](https://developer.mozilla.org/zh-CN/docs/learn/Accessibility)，因为如果它们互相独立，屏幕阅读器可以更好地阅读两个功能。</dd>
    <dt>主要内容</dt>
    <dd>中心的一个大区域，包含给定网页的大部分独特内容，例如您要观看的视频，或您正在阅读的主要故事，或您要查看的地图，或新闻标题等......这是网站的一部分，绝对会因页面而异。</dd>
    <dt>侧栏</dt>
    <dd>一些次要信息、链接、引言、广告等。通常这是与主要内容中包含的内容相关（例如在新闻文章页面上，侧边栏可能包含作者的个人信息或相关文章的链接），但有在一些情况下，你会发现一些重复的元素，如辅助导航系统。</dd>
    <dt>页脚</dt>
    <dd>横跨页面底部的条纹，通常包含精美的打印、版权通知或联系信息。 它是一个放置公共信息（如标题）的地方，但通常该信息对网站来说不是特别重要。 通过提供用于快速访问热门内容的链接，页脚有时也用于[SEO](https://developer.mozilla.org/en-US/docs/Glossary/SEO "SEO: SEO (Search Engine Optimization) is the process of making a website more visible in search results, also termed improving search rankings.")目的。</dd>
</dl>

一个“典型的网站”可能会这样布局：

![a simple website structure example featuring a main heading, navigation menu, main content, side bar, and footer.](https://mdn.mozillademos.org/files/12417/sample-website.png)

## 二、用于结构化网站的 HTML

上面显示的简单示例不是很漂亮，但是非常适合用于说明典型的网站布局。 一些网站有更多的列，有些网站更复杂，但你会有你的想法。使用正确的 CSS，您可以使用几乎任何元素来装饰不同的部分，并得到您想要的结果，但如前所述，我们需要遵守语义，并**使用正确的元素进行语义化工作**。

这是因为视觉效果并不能说明一切。 我们可以对内容最有用的部分使用颜色和字体大小来吸引用户的关注，例如导航菜单和相关链接，但是视觉障碍的人该怎么办，这难道也对那些没有“粉红色”和“大”的概念的人来说非常有用吗？

>**Note**: 色盲患者大概[占了世界人口的 8%](http://www.color-blindness.com/2006/04/28/colorblind-population/)，盲人和视障人士占世界人口的4-5％左右（2012年全球[有2.85亿人](https://en.wikipedia.org/wiki/Visual_impairment), 总人口[约70亿](https://en.wikipedia.org/wiki/World_human_population#/media/File:World_population_history.svg)）。

在您的HTML代码中，您可以根据其功能标记内容部分 - 您可以明确地使用表示上述内容部分的元素，屏幕阅读器等辅助技术可以识别这些元素，并帮助执行“找到主导航 “或”找到主要内容“。 正如我们前面提到的那样，[没有使用正确的元素结构和语义去构建网页会有很多的不良影响](https://developer.mozilla.org/zh-CN/docs/learn/HTML/Introduction_to_HTML/HTML_text_fundamentals#为什么我们需要结构化)。

为了实现这样的语义标记，HTML提供了可以用来表示这些部分的专用标签，例如：

* **标题:** [`<header>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/header "<header>元素表示一组引导性的帮助，可能包含标题元素，也可以包含其他元素，像logo、分节头部、搜索表单等。").
* **导航栏:** [`<nav>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/nav "HTML导航栏 (<nav>) 描绘一个含有多个超链接的区域，这个区域包含转到其他页面，或者页面内部其他部分的链接列表.").
* **主要内容:** [`<main>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/main "HTML Main元素()呈现了文档<body>或应用的主体部分。主体部分由与文档直接相关，或者扩展于文档的中心主题、应用的主要功能部分的内容组成。这部分内容在文档中应当是独一无二的，不包含任何在一系列文档中重复的内容，比如侧边栏，导航栏链接，版权信息，网站logo，搜索框（除非搜索框作为文档的主要功能）。"), 具有代表性的内容段落主题可以使用 [`<article>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/article "<article>元素表示文档、页面、应用或网站中的独立结构，其意在成为可独立分配的或可复用的结构，如在发布中，它可能是论坛帖子、杂志或新闻文章、博客、用户提交的评论、交互式组件，或者其他独立的内容项目。"), [`<section>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/section "HTML Section 元素 (<section>) 表示文档中的一个区域（或节），比如，内容中的一个专题组，一般来说会有包含一个标题（heading）。一般通过是否包含一个标题 (<h1>-<h6> element) 作为子节点 来 辨识每一个<section>。"), 和 [`<div>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/div "HTML <div> 元素 (或 HTML 文档分区元素) 是一个通用型的流内容容器，它在语义上不代表任何特定类型的内容，它可以被用来对其它元素进行分组，一般用于样式化相关的需求（使用 class 或 id 特性) 或者对具有相同特性的一组元素进行分组 (比如 lang)，它应该在没有任何其它语义元素可用时才使用 (比如 <article> 或 <nav>) 。") 元素.
* **侧栏:** [`<aside>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/aside "<aside> 元素表示一个和其余页面内容几乎无关的部分，被认为是独立于该内容的一部分并且可以被单独的拆分出来而不会使整体受影响。其通常表现为侧边栏或者嵌入内容。他们通常包含在工具条，例如来自词汇表的定义。也可能有其他类型的信息，例如相关的广告、笔者的传记、web 应用程序、个人资料信息，或在博客上的相关链接。"); 经常嵌套在 [`<main>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/main "HTML Main元素()呈现了文档<body>或应用的主体部分。主体部分由与文档直接相关，或者扩展于文档的中心主题、应用的主要功能部分的内容组成。这部分内容在文档中应当是独一无二的，不包含任何在一系列文档中重复的内容，比如侧边栏，导航栏链接，版权信息，网站logo，搜索框（除非搜索框作为文档的主要功能）。") 中.
* **页脚:** [`<footer>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/footer "HTML <footer> 元素表示最近一个章节内容或者根节点（sectioning root ）元素的页脚。一个页脚通常包含该章节作者、版权数据或者与文档相关的链接等信息。").

### 2.1 自主学习: 探索例子的代码

我们上面所看到的例子用下面的代码表示（你也可以在[我们的GitHub repo](https://github.com/mdn/learning-area/blob/master/html/introduction-to-html/document_and_website_structure/index.html)上找到），我们希望你看看上面的例子，然后看看下面的列表，观察哪些部分组成上面所讨论的内容（标题、导航栏、主要内容、侧栏、页脚）。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">

    <title>My page title</title>
    <link href="https://fonts.googleapis.com/css?family=Open+Sans+Condensed:300|Sonsie+One" rel="stylesheet" type="text/css">
    <link rel="stylesheet" href="style.css">

    <!-- the below three lines are a fix to get HTML5 semantic elements working in old versions of Internet Explorer-->
    <!--[if lt IE 9]>
      <script src="http://html5shiv.googlecode.com/svn/trunk/html5.js"></script>
    <![endif]-->
  </head>

  <body>
    <!-- Here is our main header that is used accross all the pages of our website -->

    <header>
      <h1>Header</h1>
    </header>

    <nav>
      <ul>
        <li><a href="#">Home</a></li>
        <li><a href="#">Our team</a></li>
        <li><a href="#">Projects</a></li>
        <li><a href="#">Contact</a></li>
      </ul>

       <!-- A Search form is another commmon non-linear way to navigate through a website. -->

       <form>
         <input type="search" name="q" placeholder="Search query">
         <input type="submit" value="Go!">
       </form>
     </nav>

    <!-- Here is our page's main content -->
    <main>

      <!-- It contains an article -->
      <article>
        <h2>Article heading</h2>

        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Donec a diam lectus. Set sit amet ipsum mauris. Maecenas congue ligula as quam viverra nec consectetur ant hendrerit. Donec et mollis dolor. Praesent et diam eget libero egestas mattis sit amet vitae augue. Nam tincidunt congue enim, ut porta lorem lacinia consectetur.</p>

        <h3>subsection</h3>

        <p>Donec ut librero sed accu vehicula ultricies a non tortor. Lorem ipsum dolor sit amet, consectetur adipisicing elit. Aenean ut gravida lorem. Ut turpis felis, pulvinar a semper sed, adipiscing id dolor.</p>

        <p>Pelientesque auctor nisi id magna consequat sagittis. Curabitur dapibus, enim sit amet elit pharetra tincidunt feugiat nist imperdiet. Ut convallis libero in urna ultrices accumsan. Donec sed odio eros.</p>

        <h3>Another subsection</h3>

        <p>Donec viverra mi quis quam pulvinar at malesuada arcu rhoncus. Cum soclis natoque penatibus et manis dis parturient montes, nascetur ridiculus mus. In rutrum accumsan ultricies. Mauris vitae nisi at sem facilisis semper ac in est.</p>

        <p>Vivamus fermentum semper porta. Nunc diam velit, adipscing ut tristique vitae sagittis vel odio. Maecenas convallis ullamcorper ultricied. Curabitur ornare, ligula semper consectetur sagittis, nisi diam iaculis velit, is fringille sem nunc vet mi.</p>
      </article>

      <!-- the aside content can also be nested within the main content -->
      <aside>
        <h2>Related</h2>

        <ul>
          <li><a href="#">Oh I do like to be beside the seaside</a></li>
          <li><a href="#">Oh I do like to be beside the sea</a></li>
          <li><a href="#">Although in the North of England</a></li>
          <li><a href="#">It never stops raining</a></li>
          <li><a href="#">Oh well...</a></li>
        </ul>
      </aside>

    </main>

    <!-- And here is our main footer that is used across all the pages of our website -->

    <footer>
      <p>©Copyright 2050 by nobody. All rights reversed.</p>
    </footer>

  </body>
</html>
```

花一些时间来查看代码并理解它 - 代码中的注释也应该帮助您理解它。 我们不要求你在这篇文章中做很多其他事情，因为理解文档布局的关键是编写一个完整的 HTML 结构，然后用CSS布局。等你学习到 CSS 部分的时候才能完全理解上面代码。

## 三、HTML布局元素细节

从总体详细的理解HTML的元素是不错的——随着你web开发经验的逐渐积累，你将会逐渐理解 HTML 的元素。你可以通过查阅[HTML元素参考](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element)找到更多的细节。现在，你需要理解这些主要的元素定义：

* [`<main>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/main "HTML Main元素()呈现了文档<body>或应用的主体部分。主体部分由与文档直接相关，或者扩展于文档的中心主题、应用的主要功能部分的内容组成。这部分内容在文档中应当是独一无二的，不包含任何在一系列文档中重复的内容，比如侧边栏，导航栏链接，版权信息，网站logo，搜索框（除非搜索框作为文档的主要功能）。") 展现了页面内容的独特性。只可以在每一个页面上使用一次<main>，直接把它放到[`<body>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/body "HTML <body> 元素表示HTML文档内容所在之处。一个文档中只允许有一个 <body> 元素。")中。在理想情况下，不应该把它嵌套进其他的元素中。
* [`<article>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/article "<article>元素表示文档、页面、应用或网站中的独立结构，其意在成为可独立分配的或可复用的结构，如在发布中，它可能是论坛帖子、杂志或新闻文章、博客、用户提交的评论、交互式组件，或者其他独立的内容项目。") 闭合一块与自身相关的内容，这块内容能够解释它自身而不是页面上其他的内容（例如一篇单独的博客）。
* [`<section>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/section "HTML Section 元素 (<section>) 表示文档中的一个区域（或节），比如，内容中的一个专题组，一般来说会有包含一个标题（heading）。一般通过是否包含一个标题 (<h1>-<h6> element) 作为子节点 来 辨识每一个<section>。") 近似于<article>，但是它更多的是伴随着由一个单独功能构成的页面（例如一个小型的地图，或者是一组文章的标题和摘要）。它被认为最好的实际应用是用[标题](https://developer.mozilla.org/en-US/Learn/HTML/Howto/Set_up_a_proper_title_hierarchy)作为每一部分（section）的开头；也要注意的是你可以把不同的<article>分到不同的<section>中，或者把不同的<section>分到不同的<article>中，这要取决于内容。
* [`<aside>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/aside "<aside> 元素表示一个和其余页面内容几乎无关的部分，被认为是独立于该内容的一部分并且可以被单独的拆分出来而不会使整体受影响。其通常表现为侧边栏或者嵌入内容。他们通常包含在工具条，例如来自词汇表的定义。也可能有其他类型的信息，例如相关的广告、笔者的传记、web 应用程序、个人资料信息，或在博客上的相关链接。") 包含的内容并不与主要内容有直接的联系，但是它可以提供额外的不直接有联系的信息（术语表条目，作者简介，相关链接等等）。
* [`<header>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/header "<header>元素表示一组引导性的帮助，可能包含标题元素，也可以包含其他元素，像logo、分节头部、搜索表单等。") 展现了一系列的介绍性内容。如果它是[`<body>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/body "HTML <body> 元素表示HTML文档内容所在之处。一个文档中只允许有一个 <body> 元素。") 的子元素,它就定义了网站的全局页眉。但是如果它是 [`<article>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/article "<article>元素表示文档、页面、应用或网站中的独立结构，其意在成为可独立分配的或可复用的结构，如在发布中，它可能是论坛帖子、杂志或新闻文章、博客、用户提交的评论、交互式组件，或者其他独立的内容项目。") 或[`<section>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/section "HTML Section 元素 (<section>) 表示文档中的一个区域（或节），比如，内容中的一个专题组，一般来说会有包含一个标题（heading）。一般通过是否包含一个标题 (<h1>-<h6> element) 作为子节点 来 辨识每一个<section>。") 的子元素，它就定义了这些部分的特定的页眉(不要把这些与[titles and headings](https://developer.mozilla.org/en-US/Learn/HTML/Introduction_to_HTML/The_head_metadata_in_HTML#Adding_a_title)混淆)。
* [`<nav>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/nav "HTML导航栏 (<nav>) 描绘一个含有多个超链接的区域，这个区域包含转到其他页面，或者页面内部其他部分的链接列表.") 包含了页面主要的导航功能。二级链接等，不会进入导航功能部分。
* [`<footer>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/footer "HTML <footer> 元素表示最近一个章节内容或者根节点（sectioning root ）元素的页脚。一个页脚通常包含该章节作者、版权数据或者与文档相关的链接等信息。") 包含了页面的页脚部分。

### 3.1 没有特定语义的装饰元素

有时候，你会遇到一种情况——你找不到理想的语义元素来包含项目或内容。有时候你可能只想仅仅用[CSS](https://developer.mozilla.org/en-US/docs/Glossary/CSS "CSS: CSS (Cascading Style Sheets) is a declarative language that controls how webpages look in the browser.")或[JavaScript](https://developer.mozilla.org/en-US/docs/Glossary/JavaScript "JavaScript: JavaScript (JS) is a programming language mostly used to dynamically script webpages on the client side, but it is also often utilized on the server-side, using packages such as Node.js.")将一组元素作为一个单独的实体来修饰。为了应对这种情况，HTML提供了[`<div>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/div "HTML <div> 元素 (或 HTML 文档分区元素) 是一个通用型的流内容容器，它在语义上不代表任何特定类型的内容，它可以被用来对其它元素进行分组，一般用于样式化相关的需求（使用 class 或 id 特性) 或者对具有相同特性的一组元素进行分组 (比如 lang)，它应该在没有任何其它语义元素可用时才使用 (比如 <article> 或 <nav>) 。")和[`<span>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/span "HTML <span> 元素是短语内容的通用行内容器，并没有任何特殊语义。可以使用它来编组元素以达到某种样式意图（通过使用类或者Id属性），或者这些元素有着共同的属性，比如lang。应该在没有其他合适的语义元素时才使用它。<span> 与 <div> 元素很相似，但 <div> 是一个 块元素 而 <span> 则是  行内元素 .")元素。你应该最好使用`[class](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes#attr-class)`属性来提供一些标签，这样他们就能容易的被找到。

[`<span>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/span "HTML <span> 元素是短语内容的通用行内容器，并没有任何特殊语义。可以使用它来编组元素以达到某种样式意图（通过使用类或者Id属性），或者这些元素有着共同的属性，比如lang。应该在没有其他合适的语义元素时才使用它。<span> 与 <div> 元素很相似，但 <div> 是一个 块元素 而 <span> 则是  行内元素 .") 是一个行内无语义元素，你应该仅仅当无法找到更好的语义元素包含内容时使用，或者不想增加特定的含义。例如：

```html
<p>The King walked drunkenly back to his room at 01:00, the beer doing nothing to aid
him as he staggered through the door <span class="editor-note">[Editor's note: At this point in the
play, the lights should be down low]</span>.</p>
```

在这种情况中，the editor’s note 应该仅仅是提供额外的对导演戏剧的说明；它没有额外的语义。对于用户来说，CSS可能用于从主文本中抽离这些note。

[`<div>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/div "HTML <div> 元素 (或 HTML 文档分区元素) 是一个通用型的流内容容器，它在语义上不代表任何特定类型的内容，它可以被用来对其它元素进行分组，一般用于样式化相关的需求（使用 class 或 id 特性) 或者对具有相同特性的一组元素进行分组 (比如 lang)，它应该在没有任何其它语义元素可用时才使用 (比如 <article> 或 <nav>) 。") 是一个块级无语义元素，你应该仅仅当找不到更好的块级元素时使用，或者不想增加特定的意义。例如，想象当你浏览一个电子商务网站时，有一个购物车部件一直都在你可以选择的地方。

```html
<div class="shopping-cart">
  <h2>Shopping cart</h2>
  <ul>
    <li>
      <p><a href=""><strong>Silver earrings</strong></a>: $99.95.</p>
      <img src="../products/3333-0985/" alt="Silver earrings">
    </li>
    <li>
      ...
    </li>
  </ul>
  <p>Total cost: $237.89</p>
</div>
```

这并不是一个 `<aside>`, 因为它和主要内容并没有必要的联系（你想在任何地方都能看到它。它甚至不能用`<section>`来特定的指定，因为它不是页面上主要内容的一部分。所以在这种情况下用`<div>`是合适的, 我们还需添加一个head标签帮助屏幕阅读者找到它。

**警告**: Divs用起来非常便利以至于很容易被滥用。因为它们不携带语义值，所以会让你的HTML代码变的混乱。要小心的使用它们，只有当没有更好的语义解决方案才能使用，而且要尽可能把它的使用量降到最低，否则，当你升级和维护你的文档时会非常困难。

### 3.2 换行与水平分割线

[`<br>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/br "HTML <br> 元素在文本中生成一个换行（回车）符号。此元素在写诗和地址时很有用，这些地方的换行都非常重要。") 和[`<hr>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/hr "HTML <hr> 元素表示段落级元素之间的主题转换（例如，一个故事中的场景的改变，或一个章节的主题的改变）。在HTML的早期版本中，它是一个水平线。现在它仍能在可视化浏览器中表现为水平线，但目前被定义为语义上的，而不是表现层面上。")将会是你偶尔使用并且想要了解的两个元素:

`<br>`在一个段落中创建一个换行；在你想要生成一系列的短行的地方，`<br>`是唯一能够生成这种结构的元素。例如一个邮寄地址或一首诗。比如：

```html
<p>There once was a girl called Nell<br>
Who loved to write HTML<br>
But her structure was bad, her semantics were sad<br>
and her markup didn't read very well.</p>
```

没有 `<br>` 元素，这一段会直接表示在一行中（正如我们之前在课程中看到的，[HTML会忽视大部分空格](https://developer.mozilla.org/zh-CN/docs/learn/HTML/Introduction_to_HTML/Getting_started#HTML中的空白)）；有了 `<br>` 元素，会生成下面这样的：

There once was a girl called Nell  
Who loved to write HTML  
But her structure was bad, her semantics were sad  
and her markup didn't read very well.

`<hr>` 元素在文档中生成一条水平分割线，表示文本中主题的变化（例如主题或场景的变化）。看起来就是一条水平线。像下面的例子：

```html
<p>Ron was backed into a corner by the marauding netherbeasts. Scared, but determined to protect his friends, he raised his wand and prepared to do battle, hoping that his distress call had made it through.</p>
<hr>
<p>Meanwhile, Harry was sitting at home, staring at his royalty statement and pondering when the next spin off series would come out, when an enchanted distress letter flew through his window and landed in his lap. He read it hasily, and lept to his feet; "better get back to work then", he mused.</p>
```

将会表示成这样：

Ron was backed into a corner by the marauding netherbeasts. Scared, but determined to protect his friends, he raised his wand and prepared to do battle, hoping that his distress call had made it through.

---

Meanwhile, Harry was sitting at home, staring at his royalty statement and pondering when the next spin off series would come out, when an enchanted distress letter flew through his window and landed in his lap. He read it hasily and sighed; "better get back to work then", he mused.

## 四、设计一个简单的网站

一旦你设计好了一个简单网站的所有内容，按照正常的逻辑思维，我们应该尝试制定出你想要放在整个网站上的 **内容**，哪些页面是你需要的，这些页面应该如何排列，以及如何互相链接，带给用户最好的使用体验。这就是所谓的 [Information architecture](https://developer.mozilla.org/en-US/docs/Glossary/Information_architecture "Information architecture: Information architecture, as applied to web design and development, is the practice of organizing the information / content / functionality of a web site so that it presents the best user experience it can, with information and services being easily usable and findable.").。在一个结构庞大、复杂的网站里，大多数设计都可以参照上述的 information architecture（信息架构），不过对于一个只有几个页面的简单网站，设计过程可以更简单，更有趣！

## 五、总结

通过本文，你应该对于如何构建一个网页/网站有了更好的理解。在本单元的最后一篇文章中，我们将学习如何调试 HTML。

## 参考：
- [文件和网站结构](https://developer.mozilla.org/zh-CN/docs/learn/HTML/Introduction_to_HTML/%E6%96%87%E4%BB%B6%E5%92%8C%E7%BD%91%E7%AB%99%E7%BB%93%E6%9E%84)
