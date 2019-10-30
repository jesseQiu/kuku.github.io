---
title: 认识字体渲染
date: 2018-05-15 13:08:00
updated: 2018-05-15 13:08:00
categories: Html
---

>在 Mac OS 系统上对网页截屏，然后打开图片不断放大观察。你会发现黑色的文字居然不完全是纯黑色的！（Windows 7下IE9使用微软雅黑字体也可以看到这个现象）。肿么回事，这不科学！读完这篇文章后你就都懂了。

![post cover](https://ksmx.me/content/images/2013/09/cover.png)

为什么相同的字体，在Mac OS上的显示效果「看起来」要比Windows上好看？这个问题我一直没有搞清楚，昨天偶尔看到有关字体渲染方面的一篇文章：[《A Closer Look At Font Rendering》](http://www.smashingmagazine.com/2012/04/24/a-closer-look-at-font-rendering/)，终于对字体渲染有了一些认识。这篇文章是我对字体渲染知识的归纳总结，只挑重点写。

## 一、一个理想和三种实现（渲染策略，Rendering Strategies）

### 1.1 理想的形状

![理想的形状（ideal shape）](https://ksmx.me/content/images/2013/09/ideal_shape.png)

理想中的文字，指的是使用矢量图形描述出来的形状。矢量图形是在计算机图形学中使用数学方程表达的几何形状（点、线和多边形等）来绘制图像。这样抽象的描述可能比较难懂，可以理解为矢量图形是「e这个字母的形状是数字9翻转过来的图形」这种描述的数学版。

如何将这种抽象形状描述转化为展示在显示器上字体呢？这需要引入一个新的术语——栅格化（Rasterization），栅格化指的是将理想中的形状转化为一个一个像素的这个过程。我们的显示器、手机屏幕实际上都是有无数个发光的像素点构成的，它们在单位面积排列得越密集显示效果越精细（PPI，像素密度）。你可能已经注意到，理想形状示意图里的字母e并不能和灰色的网格（可以理解为像素点）对应起来，尤其是曲线的边缘，只占了网格的一部分。由于我们所能控制的最小单位就是像素，便造成了理想和现实间的差距，如何让以像素为基础的屏幕更好的表达我们理想中的文字形状，牛X的前辈们发明了三种字体渲染策略，后面一一道来。


### 1.2 初代：黑白渲染（black-and-white rendering）

![黑白渲染（black-and-white rendering）](https://ksmx.me/content/images/2013/09/black_and_white_rendering.png)

黑白渲染是最早人们使用的渲染技术，这种渲染方式只使用黑白两种颜色来表达文字的形状。还记得Windows的蓝天白云界面上的文字显示效果吗？还有Dos和PC机器启动时候的提示文字，都使用了这种渲染方法。在显示屏幕上，少量的像素点并不能很好地传达字体微妙的形状变化，在圆的边缘轮廓上我们就会发现锯齿。这时字体渲染技术还处在初级阶段。

### 1.3 二代：灰度渲染（Grayscale rendering）

![灰度渲染（Grayscale rendering）](https://ksmx.me/content/images/2013/09/grayscale_rendering.png)

在上世纪90年代中期，我们的前辈们开始使用一种非常巧妙的方法，可以说是黑白渲染的优化版。灰度渲染可以控制每个像素的明暗，让字形边界看起来过渡平滑。处于字形边界上的像素亮度取决于自身被理想形状所覆盖的面积比值。这样，字体轮廓看起来就更平滑，字体设计的细节也得以再现。字体在屏幕上看起不仅清晰——而且还能体现字体本身特征及风格。我们人眼和大脑在解读灰色像素中信息时，将它转换为了形状的轮廓，因此我们感觉这样渲染后的效果更加接近原始的形状。

### 1.4 三代：亚像素渲染（Subpixel rendering）

![亚像素渲染（Subpixel rendering）](https://ksmx.me/content/images/2013/09/subpixel_rendering.png)

隆重介绍 [亚像素渲染](http://en.wikipedia.org/wiki/Subpixel_rendering)，第三代渲染技术，它一个重要特征是引入彩色像素。如果我们将屏幕截屏放大，发现字体边缘呈红色和蓝色，那么我们就可以知道它采用的是亚像素渲染技术。 有趣的是，这一渲染技术和显示技术的发展是息息相关的，在液晶显示屏（LCD）上，一个像素是由红、绿、蓝三个子（亚）像素构成的，LCD能够做到单独控制每一个子像素的开关。LCD屏幕像素在显微镜下的样子：

![LCD液晶显示屏像素构成](https://ksmx.me/content/images/2013/09/lcd_display.png)

因为这些子像素非常小，以至于人眼无法察觉到他们是一个个独立的颜色点。与单纯的灰度渲染相比，水平方向的分辨率翻了三倍。竖笔的位置及粗细就可表现的更为精确，文本外观也就更为清晰。

通过观察，我还发现亚像素渲染总是把暖色放在左边（如上图的红），冷色（如上图的蓝）放在右边，据我自己的猜想这很有可能与从左向右的阅读顺序有关。这样的设计从视觉上会感受到光源是从左边进入，增加字体的立体感，让阅读更加舒适。（这仅仅是我个人的猜测，如何有人能从色彩原理上解释这一点，请一定要告诉我）

下面图片是我在Mac OS上的Finder侧边栏截图放大，仔细观察文字后半部分能体现出这种微妙的感觉：

![Mac OS系统Finder截图放大效果](https://ksmx.me/content/images/2013/09/mac_os_screenshot.png)

## 二、在各种操作系统中的应用情况

了解完了三种字体渲染策略，我们来看一下各种操作系统对渲染方式的选择。

### 2.1 Windows系统

在Windows系统下，使用什么渲染技术取决于使用什么样的字体，以及应用程序的设置。Windows系统有两套图形文字渲染接口，一个是GDI，另一个是Windows Vista之后推出的DirectWrite，用于取代老的GDI。严格来说，微软自己的亚像素渲染技术有另一个名字——[ClearType](http://en.wikipedia.org/wiki/ClearType)，这个技术与两套图形文字渲染接口是被包含的关系。

ClearType在微软自家系统和程序中支持情况如下：

* Windows XP (off by default)
* Windows Vista (on by default)
* Windows 7 (on by default)
* Microsoft Office 2007 and later (on by default)
* Internet Explorer 7 and later (on by default)
* Windows Live Messenger (on by default)

字体渲染策略与浏览器和字体格式的关系：

![字体渲染策略与浏览器和字体格式的关系](https://ksmx.me/content/images/2013/09/font_rendering_browser_support.png)

PS-webfonts指的是PostScript字体，TrueType 和 PostScript 区别在于在描绘曲线时所有的数学方法不一样，只有字体设计人员才需要了解两者的区别。感兴趣想要深入了解可以自己Google资料学习。

Windows系统为了兼容性，提供了大量的选择权给应用程序和系统，最终的结果是在Windows系统上的文字阅读体验不够统一，有的地方看上去不错，有的却无法直视。这和Windows系统整体的生态环境是息息相关的，它需要支持各种各样的屏幕和分辨率，再加上我们之前提到的亚像素渲染技术是和LCD液晶屏幕的物理属相息息相关的一项技术，不同的LCD像素排布都需要做相应的适配，加大微软完善这一体验的成本。

### 2.2 Mac OS 系统

在Mac OS系统上，所有浏览器使用的是Quartz渲染引擎，这个引擎据说非常可靠。TrueType和PostScript字体都是以同样的方式渲染的，也就是使用了亚像素渲染技术，渲染提示（hinting）被故意忽略了，而这正是两类字体在概念上最大的差别，Mac OS对PS和TrueType字体同等对待，不管它们那些不一样的特性。Mac OS的字体渲染技术不会试图理解构成字体的笔画及特征。字母形状不会解读，因而也就不会出现曲解的情况。另外苹果似乎也会应用一些精妙的自动化措施增强渲染效果，但是这类自增强的技术没有文档说明，我们无从得知这背后的细节。在Mac 上另外碰到的一个难以控制的现象是，字体会渲染的更重些。在文本字体大小下，这点差异尤其明显。同样的字体在Mac OS上看起来有点浓稠，而在Windows上则看起要清淡些。

Mac OS系统只运行在自己的Mac上，Apple在字体渲染方面更容易给用户一个统一的体验，再一次体现了软硬结合的优势。

## 三、总结

「Mac上字体看着比Windows好看」其实是几年前Windows XP时代的刻板映像。 应该来说，在不同操作系统上，字体渲染技术的区别正在缩小，随着微软DirectWirte的普及，Windows和Mac OS在字体渲染方面的区别仅在于他们对字体显示的理解和偏好。Joel Spolsky 的 [《Font smoothing, anti-aliasing, and sub-pixel rendering》](http://www.joelonsoftware.com/items/2007/06/12.html) 一文解释了两者的区别。

> 苹果总体上认为，（字体渲染）算法的目标应尽可能还原字体的设计，即使代价是造成些许模糊。

> 微软认为，字符的形状应和像素契合，以防止模糊，提高可读性，即便扭曲了字体的构造。

更令人惊叹的是，针对LCD优化的Subpixel rendering技术，居然在1977年的Apple II 的图形显示中就已经被发明了，比微软宣布ClearType技术整整早了22年。Steve Wozniak，Apple II的设计师和这项专利的拥有者对究竟是谁先发明了这项技术的回复如下：

>"Back in 1976, my design of the Apple II's high resolution graphics system utilized a characteristic of the NTSC color video signal (called the 'color subcarrier') that creates a left to right horizontal distribution of available colors. By coincidence, this is exactly analogous to the R-G-B distribution of colored sub-pixels used by modern LCD display panels. So more than twenty years ago, Apple II graphics programmers were using this 'sub-pixel' technology to effectively increase the horizontal resolution of their Apple II displays.”

具体的争论细节可以八卦 [Sub-Pixel Font Rendering Technology - Who Di It First](https://www.grc.com/ctwho.htm)。

虽然当时这项技术是运用在NTSC信号上的，但技术的思路都是一致的。乔帮主当时是有一个多牛X的技术团队呀，对帮主的敬畏有多出了几分！

最后，给大家留个悬念，在iPhone或者安卓手机上找一个网页打开截屏，图片拿到电脑上不断放大后，你会发现手机上的字体渲染并没有使用亚像素渲染，这是为什么呢？^_^ 答案留给你去寻找吧。


## 参考：
- [认识字体渲染](https://ksmx.me/understanding-font-rendering/)