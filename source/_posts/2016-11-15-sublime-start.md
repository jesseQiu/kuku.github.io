---
title: Sublime Start
date: 2016-11-15 11:19:05
updated: 2016-11-15 11:19:05
tags: Sublime
categories: Tools
feature:
---

> Sublime Text 是一个代码编辑器,也是 HTML 和散文先进的文本编辑器。Sublime Text 是由程序员 Jon Skinner 于 2008 年 1 月份所开发出来，它最初被设计为一个具有丰富扩展功能的 Vim.

### 一、软件安装
官网下载地址 [Sublime Text 3](http://www.sublimetext.com/3)

### 二、Packager Controller 安装
Sublime Text 的插件是非常丰富的，安装插件最方便的方法就是能过 Package Control，但在 Sublime Text 中，默认是没有安装 Package Control 的，那 Package Control 要怎么安装呢？非常简单，下面为大家分享方法

- 打开 [Packager Controler](https://packagecontrol.io/) 官网
- 点击右侧的 Install Now 按钮
- 在打开的页面中就能看到 Sublime Text 3 和 Sublime Text 2 的安装代码
- 打开 Sublime Text 控制台: View -> Show Console (快捷键: Ctrl + `)
- 在控制台命令输入栏，粘贴复制好的 Package Control 安装代码，回车运行
- 安装好 Package Control 后，可以在 Sublime Text 中，按 "Ctrl + Shift + P" 打开命令面板，输入 "pc"，就可以显示 Package Control 的一些命令，如：install / remove 等

### 三、常用插件
1. Emmet 插件
功能：编码快捷键，前端必备
简介：Emmet 作为 zen coding的升级版，对于前端来说，可是必备插件，如果你对它还不太熟悉，可以在其官网（http://docs.emmet.io/）上看下具体的演示视频。
使用：教程
http://docs.emmet.io/cheat-sheet/ 
http://peters-playground.com/Emmet-Css-Snippets-for-Sublime-Text-2/

2. JsFormat
功能：Javascript的代码格式化插件
简介：很多网站的JS代码都进行了压缩，一行式的甚至混淆压缩，这让我们看起来很吃力。而这个插件能帮我们把原始代码进行格式的整理，包括换行和缩进等等，是代码一目了然，更快读懂~
使用：在已压缩的JS文件中，右键选择jsFormat或者使用默认快捷键（Ctrl+Alt+F）

3. ConvertToUtf8
功能：文件转码成utf-8
简介：通过本插件，您可以编辑并保存目前编码不被 Sublime Text 支持的文件，特别是中日韩用户使用的 GB2312，GBK，BIG5，EUC-KR，EUC-JP ，ANSI等。ConvertToUTF8 同时支持 Sublime Text 2 和 3。
使用：安装插件后自动转换为utf-8格式

4. DocBlockr
功能：生成优美注释
简介：标准的注释，包括函数名、参数、返回值等，并以多行显示，手动写比较麻烦
使用：输入/*、/**然后回车，还有很多用法，请参照
https://sublime.wbond.net/packages/DocBlockr

5. AutoFileNme
功能：快捷输入文件名
简介：自动完成文件名的输入，如图片选取
使用：输入”/”即可看到相对于本项目文件夹的其他文件
x. NodeJs
功能：node代码提示
教程：https://sublime.wbond.net/packages/Nodejs

6. AngularJs
它是有 Angular-UI 团队开发的，可能是列表中比较偏大（但是是必须的）插件，它的功能包括：
	- AngularJS 核心指令的代码补全功能
	- 自定义指令的指令完成
	- directives, controllers and filters的快速搜索
	- Angular相关的代码片段
	- GoToDocs for core AngularJS directives

7. HTML+CSS+JAVASCRIPT+JSON 快速格式化
	a. 在 Sublime Text 中，按下 Ctrl+Shift+P 调出命令面板;
	b. 输入install 调出 Install Package 选项并回车;
	c. 输入 pretty，并在列表中选择 HTML-CSS-JS Prettify 后回车即可安装
	d. 快捷键: Ctrl+Shift+H
	e. 注意：改插件依赖 nodejs
	f. 参考：http://jokerliang.com/html-css-javascript-and-json-code-formatter-for-sublime-text-2-and-3.html
	
8. Markdown Preview
这个插件提供了标准的 markdown 语法支持，而且可以提供基础 markdown 和 github 两种格式的预览，并且可以使用 `ctrl + b（command + b）` 立即生成 html 文件

9. AllAutocomplete
传统的 Sublime Text 自动补全插件仅仅在当前文件下工作。AllAutocomplete 可以搜索全部打开的标签页，这将极大的简化开发进程。当然，还有一个插件叫 CodeIntel，实现了一些 IDE 的功能并且为一些语言提供了“代码情报”： JavaScript, Mason, XBL, XUL, RHTML, SCSS, Python, HTML, Ruby, Python3, XML, Sass, XSLT, Django, HTML5, Perl, CSS, Twig, Less, Smarty, Node.js, Tcl, TemplateToolkit, PHP.

### 四、常用快捷键
```bash
Ctrl+Shift+P：打开命令面板
Ctrl+P：搜索项目中的文件
Ctrl+G：跳转到第几行
Ctrl+W：关闭当前打开文件
Ctrl+Shift+W：关闭所有打开文件
Ctrl+Shift+V：粘贴并格式化
Ctrl+D：选择单词，重复可增加选择下一个相同的单词
Ctrl+L：选择行，重复可依次增加选择下一行
Ctrl+Shift+L：选择多行
Ctrl+Shift+Enter：在当前行前插入新行
Ctrl+X：删除当前行
Ctrl+M：跳转到对应括号
Ctrl+U：软撤销，撤销光标位置
Ctrl+J：选择标签内容
Ctrl+F：查找内容
Ctrl+Shift+F：查找并替换
Ctrl+H：替换
Ctrl+R：前往 method
Ctrl+N：新建窗口
Ctrl+K+B：开关侧栏
Ctrl+Shift+M：选中当前括号内容，重复可选着括号本身
Ctrl+F2：设置/删除标记
Ctrl+/：注释当前行
Ctrl+Shift+/：当前位置插入注释
Ctrl+Alt+/：块注释，并Focus到首行，写注释说明用的
Ctrl+Shift+A：选择当前标签前后，修改标签用的
F11：全屏
Shift+F11：全屏免打扰模式，只编辑当前文件
Alt+F3：选择所有相同的词
Alt+.：闭合标签
Alt+Shift+数字：分屏显示
Alt+数字：切换打开第N个文件
Shift+右键拖动：光标多不，用来更改或插入列内容
鼠标的前进后退键可切换Tab文件
按Ctrl，依次点击或选取，可需要编辑的多个位置
按Ctrl+Shift+上下键，可替换行
```

### 五、使用技巧
- 菜单栏不见了
	a. Ctrl + Shift + p -> view -> toggle menu
	b. 开关其它模块的显示/隐藏也是用类似方法

- [exec 表格转 HTML](http://pressbin.com/tools/excel_to_html_table/index.html) 

### 参考
- [安装 Packger Controler](http://jingyan.baidu.com/article/f71d60379b20071ab641d181.html)
- [sublime 推荐必备插件](http://www.xuanfengge.com/practical-collection-of-sublime-plug-in.html)
- [JavaScript 开发者必备的 10 款 SublimeText 插件](http://linux.it.net.cn/e/news/2015/0904/16932.html)
- [推荐！Sublime Text 最佳插件列表](http://blog.jobbole.com/79326/)
- [关于Sublime Text 3](http://www.cnblogs.com/hykun/p/sublimeText3.html)
