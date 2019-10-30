---
title: 怎样把图片嵌入到 HTML 中(十)
date: 2018-05-01 12:07:00
updated: 2018-05-01 12:07:00
categories: Html
---

>现在我们可以轻松的为一张 web 网页添加简单的图像，下一步是开始为 HTML 文档添加音频和视频的播放器。在这篇文章当中，我们会学习到 [`<video>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video "HTML <video> 元素 用于在HTML或者XHTML文档中嵌入视频内容。") 和 [`<audio>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/audio "HTML <audio> 元素用于在文档中表示音频内容。 <audio> 元素可以包含多个音频资源， 这些音频资源可以使用 src 属性或者<source> 元素来进行描述； 浏览器将会选择最合适的一个来使用。对于不支持<audio>元素的浏览器，<audio>元素也可以作为浏览器不识别的内容加入到文档中。") 两个标签；然后我们还将会看看如何为你的视频添加字幕。

>前提: | 基础计算机能力，[基础的软件安装](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/Installing_basic_software)，基础的[文件处理](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/Dealing_with_files)知识，基础的 HTML 知识 (包括 [Getting started with HTML](https://developer.mozilla.org/zh-CN/docs/learn/HTML/Introduction_to_HTML/Getting_started) ) 以及 [Images in HTML](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Multimedia_and_embedding/Images_in_HTML).

目标: | 学习如何在一张 web 页面中嵌入音频和视频，以及如何为视频添加字幕。

## 一、web 中的音频和视频

web 开发者们一直以来想在 Web 中使用音频和视频，自 21 世纪初以来，我们的带宽开始能够支持任意类型的视频（视频文件比文本和图片要大的多）。在早些时候，传统的 web 技术（如 HTML ）不能够在 Web 中嵌入音频和视频，所以一些像 [Flash](https://en.wikipedia.org/wiki/Adobe_Flash) (后来有 [Silverlight](https://en.wikipedia.org/wiki/Microsoft_Silverlight) ) 的专利技术在处理这些内容上变得很受欢迎。这些技术能够正常的工作，但是却有着一系列的问题，包括无法很好的支持 HTML/CSS 特性、安全问题，以及可行性问题。

传统的解决方案能够解决许多这样的问题，前提是它能够正确的工作。幸运的是，几年之后 [HTML5](https://developer.mozilla.org/en-US/docs/Glossary/HTML5 "HTML5: The latest stable release of HTML, HTML5 takes HTML from a simple markup for structuring a document to a full app development platform. Among other features, HTML5 includes new elements and JavaScript APIs to enhance storage, multimedia, and hardware access.") 标准提出，其中有许多的新特性，包括 [`<video>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video "HTML <video> 元素 用于在HTML或者XHTML文档中嵌入视频内容。") 和 [`<audio>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/audio "HTML <audio> 元素用于在文档中表示音频内容。 <audio> 元素可以包含多个音频资源， 这些音频资源可以使用 src 属性或者<source> 元素来进行描述； 浏览器将会选择最合适的一个来使用。对于不支持 <audio> 元素的浏览器，<audio>元素也可以作为浏览器不识别的内容加入到文档中。") 标签，以及一些 [JavaScript](https://developer.mozilla.org/en-US/docs/Glossary/JavaScript "JavaScript: JavaScript (JS) is a programming language mostly used to dynamically script webpages on the client side, but it is also often utilized on the server-side, using packages such as Node.js.") 和 [APIs](https://developer.mozilla.org/en-US/docs/Glossary/API "APIs: An API (Application Programming Interface) is a set of features and rules that exist inside a software program enabling interaction between the software and other items, such as other software or hardware.") 用于对其进行控制。在这里，我们不讨论有关 JavaScript 的问题，仅仅讲解有关 HTML 的基础。

我们不会教你如何制作音频和视频，因为那需要一种完全不同的技术。我们已经为你提供了一些视频和音频的文件（ [sample audio and video files and example code](https://github.com/mdn/learning-area/tree/master/html/multimedia-and-embedding/video-and-audio-content) ）用于你进行试验，以防止你没有可进行试验的样本。

**Note**: 在你开始之前，你应当了解一些 [OVPs](https://developer.mozilla.org/zh-CN/docs/Glossary/OVP "The definition of that term (OVPs) has not been written yet; please consider contributing it!") (在线视频提供商) 例如 [YouTube](https://www.youtube.com/) 、[Dailymotion](http://www.dailymotion.com/) 、 [Vimeo](https://vimeo.com/), 以及在线音频提供商例如 [Soundcloud](https://soundcloud.com/)。这些公司提供方便、简单的方式来支持视频，所以你不必担心庞大的带宽消耗。OVPS 甚至提供现成的代码用于为你的 web 网页嵌入视频/音频。如果你使用那种服务，你便可以避免在这篇文章中我们将讨论的一些难题。在下一篇文章中，我们将会讨论这种服务。 

### 1.1 `<video>` 标签

[`<video>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video "HTML <video> 元素 用于在HTML或者XHTML文档中嵌入视频内容。") 允许你简单的嵌入一段视频。一个简单的例子如下：

```html
<video src="rabbit320.webm" controls>
  <p>Your browser doesn't support HTML5 video. Here is a <a href="rabbit320.webm">link to the video</a> instead.</p> 
</video>
```

当中的一些属性如下:
- `[src](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video#attr-src)` 同 [`<img>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img "HTML Image 元素（ <img> ）代表文档中的一个图像。") 标签使用方式相同，src 属性指向你想要嵌入网页当中的视频资源，他们的使用方式完全相同。
- `[controls](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video#attr-controls)` 用户必须能够控制视频和音频的回放功能。你可以使用浏览器提供的控制接口，同时你也可以在 JavaScript （[JavaScript API](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement)）当中使用这些控制接口。至少，这些媒体应该包括开始和停止，以及调整音量的功能。
- `<video>` 标签内的段落
这个叫做**后备内容** — 当浏览器不支持 `<video>` 标签的时候，它将会显示出来，它使我们能够对旧的浏览器做一些兼容处理。你可以添加任何后备内容，在这个例子中我们提供了一个指向这个视频文件的链接，从而使用户可以至少访问到这个文件，而不会局限于浏览器的支持。


已嵌入视频文件的网页样式如下：

![A simple video player showing a video of a small white rabbit](https://mdn.mozillademos.org/files/12794/simple-video.png)

你可以点击[这里](http://mdn.github.io/learning-area/html/multimedia-and-embedding/video-and-audio-content/simple-video.html)查看网页，或者点击[这里](https://github.com/mdn/learning-area/blob/master/html/multimedia-and-embedding/video-and-audio-content/simple-video.html)查看源代码。

### 1.2 多格式支持

以上的例子中有一个问题，你可能已经注意到了，如果你尝试使用像 Safari 或者 Internet Explorer 这些浏览器来访问上面的链接。视频并不会播放，这是因为不同的浏览器对视频格式的支持不同。

我们先来快速的了解一下术语。像 MP3、MP4、WebM 这些术语叫做**容器格式**。他们是用不同的方式来播放音频或者视频的 — 也就是说这些容器是用不同的音频轨道、视频轨道、元数据来呈现媒体文件的。

视频和音频都有不同的格式，如下:

* WebM 容器通常包括了 Ogg Vorbis 音频和 VP8/VP9 视频。主要在 FireFox 和 Chrome 当中支持。
* MP4 容器通常包括 AAC 以及 MP3 音频和 H.264 视频。主要在 Internet Explorer 和 Safari 当中支持。
* 老式的 Ogg 容器往往支持 Ogg Vorbis  音频和 Ogg Theora 视频。主要在 Firefox 和 Chrome 当中支持，不过这个容器已经被更强大的 WebM 容器所取代。

音频播放器将会直接播放音频文件,，例如 MP3 和 Ogg 文件。这些不需要容器。

**Note**: 这并没有那么简单，你可以从这里看到 [audio-video codec compatibility table](https://developer.mozilla.org/en-US/docs/Web/HTML/Supported_media_formats#Browser_compatibility).。此外，许多移动平台的浏览器能够播放一些不支持的格式，但是它们用的却是底层系统的媒体播放器。但这也仅是现在支持。

以上的格式主要用于将音频和视频压缩成可管理的文件（原始的音频和视频文件非常大）。浏览器包含了不同的 **[Codecs](https://developer.mozilla.org/en-US/docs/Glossary/Codec "Codecs: A codec  (a blend word derived from "coder-decoder") is a program, algorithm, or device that encodes or decodes a data stream. A given codec knows how to handle a specific encoding or compression technology.")**,，如 Vorbis 和 H.264,，它们用来将已压缩的音频和视频转化成二进制数字。正如刚才所说，浏览器并不全支持相同的 codecs，所以你得使用几个不同格式的文件来兼容不同的浏览器。如果你使用的格式都得不到浏览器的支持，那么媒体文件将不会播放。

**Note:** 你也许会疑惑为什么会有这样的情况存在。**MP3**  (音频格式) 和 **MP4/H.264** (视频格式) 是被广泛支持的两种格式，并且质量良好。然而，他们却有专利的阻碍 — MP3 的专利会持续到 2017 年（就在我翻译这篇文章的当天，MP3 专利解除了），而 H.264 会持续到 2027 年早期。意思也就是说浏览器若想要支持这些格式，就得支付高额的费用。此外，许多人反对软件技术垄断，支持开源的格式。这就是为什么我们需要准备不同的格式来兼容不同的浏览器。

我们该怎么做呢？请看如下例子（你可以点击这里[查看](http://mdn.github.io/learning-area/html/multimedia-and-embedding/video-and-audio-content/multiple-video-formats.html)网页，或者点击这里[查看](https://github.com/mdn/learning-area/blob/gh-pages/html/multimedia-and-embedding/video-and-audio-content/multiple-video-formats.html)源代码）：

```html
<video controls>
  <source src="rabbit320.mp4" type="video/mp4">
  <source src="rabbit320.webm" type="video/webm">
  <p>Your browser doesn't support HTML5 video. Here is a <a href="rabbit320.mp4">link to the video</a> instead.</p>
</video>
```

现在我们将 src 属性从 `<video>` 标签中移除，转而将它放在几个单独的标签 [`<source>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/source "The HTML <source> element specifies multiple media resources for either the <picture>, the <audio> or the <video> element. It is an empty element. It is commonly used to serve the same media content in multiple formats supported by different browsers.") 当中。在这个例子当中，浏览器将会检查 `<source>`标签，并且播放第一个与其自身 codec 向匹配的媒体。你的视频应当包括 WebM 和 MP4 两种格式，这两种在目前已经足够支持大多数平台和浏览器。

每个 `<source>` 标签页含有一个 type 属性，这个属性是可选的，但是建议你添加上这个属性 — 它包含了视频文件的 [MIME types](https://developer.mozilla.org/en-US/docs/Glossary/MIME_type "MIME types: A MIME type (now properly called "media type", but also sometimes "content type") is a string sent along with a file indicating the type of the file (describing the content format, for example, a sound file might be labeled audio/ogg, or an image file image/png).") ，同时浏览器也会通过检查这个属性来迅速的跳过那些不支持的格式。如果你没有添加 type 属性，浏览器会尝试加载每一个文件，直到找到一个能正确播放的格式，这样会消耗掉大量的时间和资源。

**Note**: 你可以在这里（[article on supported media formats](https://developer.mozilla.org/en-US/docs/Web/HTML/Supported_media_formats)）查看有关 [MIME types](https://developer.mozilla.org/en-US/docs/Glossary/MIME_type "MIME types: A MIME type (now properly called "media type", but also sometimes "content type") is a string sent along with a file indicating the type of the file (describing the content format, for example, a sound file might be labeled audio/ogg, or an image file image/png).") 的支持。

### 1.3 其他 `<video>` 特性

这里有许多你可以用在 HTML5 `<video>` 上的特性，请看我们的第三个例子：

```html
<video controls width="400" height="400"
       autoplay loop muted
       poster="poster.png">
  <source src="rabbit320.mp4" type="video/mp4">
  <source src="rabbit320.webm" type="video/webm">
  <p>Your browser doesn't support HTML5 video. Here is a <a href="rabbit320.mp4">link to the video</a> instead.</p>
</video>
```

这串代码将会给我们呈现出如下页面：

![A video player showing a poster image before it plays. The poster image says HTML5 video example, OMG hell yeah!](https://mdn.mozillademos.org/files/12796/extra-video-features.png)新的特性：
- `[width](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video#attr-width)` 和 `[height](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video#attr-height)` 你可以用属性控制视频的尺寸，也可以用 [CSS](https://developer.mozilla.org/en-US/docs/Glossary/CSS "CSS: CSS (Cascading Style Sheets) is a declarative language that controls how webpages look in the browser.") 来控制视频尺寸。 无论使用哪种方式，视频都会保持它原始的长宽比 — 也叫做**纵横比**。如果你设置的尺寸没有保持视频原始长宽比，那么视频边框将会拉伸，而未被视频内容填充的部分，将会显示默认的背景颜色。
- `[autoplay](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video#attr-autoplay)`
这个属性会使音频和视频内容立即播放，即使页面的其他部分还没有加载完全。建议不要应用这个属性在你的网站上，因为用户们会比较反感自动播放的媒体文件。</dd>
- `[loop](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video#attr-loop)`这个属性可以让音频或者视频文件循环播放。同样不建议使用，除非有必要。</dd>
- `[muted](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video#attr-muted)`
这个属性会导致媒体播放时，默认关闭声音。
- `[poster](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video#attr-poster)`
这个属性指向了一个图像的URL，这个图像会在视频播放前显示。通常用于粗略的预览或者广告。</dd>
- `[preload](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video#attr-preload)`
这个属性被用来缓冲较大的文件，有 3 个值可选：
	* `"none"` ：不缓冲
	* `"auto"` ：页面加载后缓存媒体文件
	* `"metadata"` ：仅缓冲文件的元数据

你可以点击[这里](https://mdn.github.io/learning-area/html/multimedia-and-embedding/video-and-audio-content/extra-video-features.html)查看以上的例子，也可以点击[这里](https://github.com/mdn/learning-area/blob/gh-pages/html/multimedia-and-embedding/video-and-audio-content/extra-video-features.html)查看源代码。注意我们并没有使用 autoplay 属性在这个版本的例子中 — 如果当页面一加载就开始播放视频的话，就不会看到 poster 属性的效果了。

### 1.4 `<audio>` 标签

[`<audio>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/audio "HTML <audio> 元素用于在文档中表示音频内容。 <audio> 元素可以包含多个音频资源， 这些音频资源可以使用 src 属性或者<source> 元素来进行描述； 浏览器将会选择最合适的一个来使用。对于不支持<audio>元素的浏览器，<audio>元素也可以作为浏览器不识别的内容加入到文档中。") 标签与 [`<video>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video "HTML <video> 元素 用于在HTML或者XHTML文档中嵌入视频内容。") 标签的使用方式几乎完全相同，有一些细微的差别比如下面的边框不同，一个典型的例子如下：

```html
<audio controls>
  <source src="viper.mp3" type="audio/mp3">
  <source src="viper.ogg" type="audio/ogg">
  <p>Your browser doesn't support HTML5 audio. Here is a <a href="viper.mp3">link to the audio</a> instead.</p>
</audio>
```

这串代码将会产生如下的效果：

![A simple audio player with a play button, timer, volume control, and progress bar](https://mdn.mozillademos.org/files/12798/audio-player.png)

**Note**: 你可以点击这里[查看](http://mdn.github.io/learning-area/html/multimedia-and-embedding/video-and-audio-content/multiple-audio-formats.html)以上例子，或者点击[这里](https://github.com/mdn/learning-area/blob/gh-pages/html/multimedia-and-embedding/video-and-audio-content/multiple-audio-formats.html)查看源代码。

音频播放器所占用的空间比视频播放器要小，由于它没有视觉部件 — 你只需要显示出能控制音频播放的控件。一些与 HTML5 `<video>` 的差异如下：

* [`<audio>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/audio "HTML <audio> 元素用于在文档中表示音频内容。 <audio> 元素可以包含多个音频资源， 这些音频资源可以使用 src 属性或者<source> 元素来进行描述； 浏览器将会选择最合适的一个来使用。对于不支持<audio>元素的浏览器，<audio>元素也可以作为浏览器不识别的内容加入到文档中。") 标签不支持 width/height 属性 — 由于其并没有视觉部件，也就没有可以设置 width/height 的内容。
* 同时也不支持 poster 属性 — 同样，没有视觉部件。

除此之外，`<audio>` 标签支持所有 `<video>` 标签拥有的特性 — 你可以回顾上面的章节来了解更多的有关信息。

## 二、显示音轨文本

现在，我们将讨论一个略微先进的概念，这个概念将会十分的有用。许多人不想（或者不能）听到 Web 上的音频/视频内容，至少在某些情况下是这样的，比如：

* 许多人患有听觉障碍（通常来说是很难听清声音的人，或者聋人），所以他们不能听见音频中的声音。
* 另外的情况可能是由于人们并不能听音频，可能是因为他们在一个非常嘈杂的环境当中（比如在一个拥挤的酒吧内恰好赶上了球赛 ），也可能是由于他们并不想打扰到其他人（比如在一个十分安静的地方，例如图书馆）。
* 有一些人他们不说音频当中的语言，所以他们听不懂，因此他们想要一个副本或者是翻译来帮助他们理解媒体内容。

给那些听不懂音频语言的人们提供一个音频内容的副本岂不是一件很棒的事情吗？所以，感谢 HTML5 `<video>` 使之成为可能，有了 [WebVTT](https://developer.mozilla.org/en-US/docs/Web/API/Web_Video_Text_Tracks_Format) 格式，你可以使用 [`<track>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/track "HTML <track> 元素 被当作媒体元素—<audio> 和 <video>的子元素来使用。它允许指定计时字幕（或者基于事件的数据），例如自动处理字幕。") 标签。

**Note**: “副本”的意思是指，用文本记录下音频的内容。

WebVTT 是一个格式，用来编写文本文件，这个文本文件包含了众多的字符串，这些字符串会带有一些元数据，它们可以用来描述这个字符串将会在视频中显示的时间，甚至可以用来描述这些字符串的样式以及定位信息。这些字符串叫做 **cues** ，你可以根据不同的需求来显示不同的样式，最常见的如下：
- subtitle: 通过添加翻译字幕，来帮助那些听不懂外国语言的人们理解音频当中的内容。
- captions: 同步翻译对白，或是描述一些有重要信息的声音，来帮助那些不能听音频的人们理解音频中的内容。
- timed descriptions: 将文字转换为音频，用于服务那些有视觉障碍的人。

一个典型的 WebVTT 文件如下：

```text
WEBVTT

1
00:00:22.230 --> 00:00:24.606
This is the first subtitle.

2
00:00:30.739 --> 00:00:34.074
This is the second.

  ...
```

让其与 HTML 媒体一起显示，你需要做如下工作：

1.  以 .vtt 后缀名保存文件。
2.  用 [`<track>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/track "HTML <track> 元素 被当作媒体元素—<audio> 和 <video>的子元素来使用。它允许指定计时字幕（或者基于事件的数据），例如自动处理字幕。") 标签链接 .vtt 文件， `<track>` 标签需放在 `<audio>` 或 `<video> 标签当中`，同时需要放在所有 `<source>` 标签之后。使用 `[kind](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/track#attr-kind)` 属性来指明是哪一种类型，如 subtitles 、 captions 、 descriptions。然后，使用 `[srclang](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/track#attr-srclang)` 来告诉浏览器你是用什么语言来编写的 subtitles。

如下:

```html
<video controls>
    <source src="example.mp4" type="video/mp4">
    <source src="example.webm" type="video/webm">
    <track kind="subtitles" src="subtitles_en.vtt" srclang="en">
</video>
```

上面这串代码会显示一段带有字幕的视频，如下：

![Video player with stand controls such as play, stop, volume, and captions on and off. The video playing shows a scene of a man holding a spear-like weapon, and a caption reads "Esta hoja tiene pasado oscuro."](https://mdn.mozillademos.org/files/7887/video-player-with-captions.png)

如果你想了解更多细节，你可以阅读 [Adding captions and subtitles to HTML5 video](https://developer.mozilla.org/en-US/Apps/Build/Audio_and_video_delivery/Adding_captions_and_subtitles_to_HTML5_video)。在 Github 上你可以找到与本文相关的样例，他们由 Ian Devlin 编写，点击[这里](http://iandevlin.github.io/mdn/video-player-with-captions/)可以查看该样例，或者点击[这里](https://github.com/iandevlin/iandevlin.github.io/tree/master/mdn/video-player-with-captions)查看源代码。这个样例使用了 JavaScript 代码，它使得用户可以选择不同的字幕。注意，若想要显示字幕，你需要点击 "CC" 按钮，并且选择一种语言 — English, Deutsch, 或 Español。

**Note**: 文本轨道会使你的网站更容易被搜索引擎抓取到 （[SEO](https://developer.mozilla.org/en-US/docs/Glossary/SEO "SEO: SEO (Search Engine Optimization) is the process of making a website more visible in search results, also termed improving search rankings.")）， 由于搜索引擎的文本抓取能力非常强大，使用文本轨道甚至可以让搜索引擎通过视频的内容直接链接。

## 三、实践学习：在你的网站上嵌入你自己的视频或音频。

在这个实践学习当中，我们希望你能够走出去，并且记录一些你自己的视频或者音频 — 如今，大多数手机都能够非常方便的记录视频或者音频，并且你可以将他们上传到你的电脑上面，你可以使用这些功能来记录你的视频或音频。在这时候，你可能需要做一些格式转换，如果是视频的话，你需要将它们转化为 WebM 或者 MP4 ，如果是音频的话，你需要将它们转化为 MP3 或者 Ogg 。 不过你并不需要担心，有许多的程序都能够帮你解决这些问题，例如 [Miro Video Converter](http://www.mirovideoconverter.com/) 和 [Audacity](https://sourceforge.net/projects/audacity/).。我们非常希望你能够亲自动手实现它。

如果你无法取得任意的音频或者视频，你可以使用我们已经为你提供的样本（[sample audio and video files](https://github.com/mdn/learning-area/tree/master/html/multimedia-and-embedding/video-and-audio-content)）。同时你也可以使用我们的代码来作为参考。

我们希望你能够：

1.  将你的音频或者视频文件保存在你电脑上的一个新目录中。
2.  创建一个新的 HTML 文件在相同的路径下，命名为 index.html。
3.  在页面上添加 `<audio>` 和 `<video>` 标签；并使用浏览器默认的控件来显示它们。
4.  在当中添加 `<source>` 标签，并添加 `type` 属性，以便于浏览器能够找到其能够支持的格式并加载它。
5.  在 `<video>` 标签中添加 poster 属性，这会显示在视频播放之前。

另外，你可以尝试研究一下文本音轨，试着为你的视频添加一些字幕。

## 四、总结

我们祝愿你可以沉浸在 Web 网站的音频和视频当中，下一篇文章，我们将会学习到另外一种在 web 页面中嵌入内容的方法，比如使用 [`<iframe>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe "HTML内联框架元素 <iframe> 表示嵌套的浏览上下文，有效地将另一个HTML页面嵌入到当前页面中。在HTML 4.01中，文档可能包含头部和正文，或头部和框架集，但不能包含正文和框架集。但是，<iframe>可以在正常的文档主体中使用。每个浏览上下文都有自己的会话历史记录和活动文档。包含嵌入内容的浏览上下文称为父浏览上下文。顶级浏览上下文（没有父级）通常是浏览器窗口。") 或者 [`<object>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/object "HTML <object> 元素（或者称作 HTML 嵌入对象元素）表示引入一个外部资源，这个资源可能是一张图片，一个嵌入的浏览上下文，亦或是一个插件所使用的资源。")。

## 参考：
- [音频和视频内容](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Multimedia_and_embedding/Video_and_audio_content)