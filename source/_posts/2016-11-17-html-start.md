---
title: Html Start
date: 2016-11-17 10:23:34
updated: 2016-11-17 10:23:34
tags: 
categories: Html
feature:
---

## 一、概述
html 全称为 HyperText Markup Language，译为 **超文本标记语言**，不是一种编程语言，是一种描述性的标记语言，用于描述超文本中内容的显示方式。比如字体什么颜色，大小等。
- 超文本：音频，视频，图片称为超文本。
- 标记 ：<英文单词或者字母>称为标记，一个 HTML 页面都是由各种标记组成。
- 作用：编写 HTML 页面。

注意：HTML 语言不是一个编程语言(有编译过程)，而是一个标记语言(没有编译过程)，HTML 页面直接由浏览器解析执行

## 二、发展历史

![html-history](http://7sby7r.com1.z0.glb.clouddn.com/2015-10-01-cnblogs_html166440-5b780bdc6b80a5fb.png?_=4850684)

## 三、html 元素脑图
![html-elements](http://od6sd4xau.bkt.clouddn.com/html-elements.png)


## 四、元素详解
1. HTML 标签不区分大小写，`<h1> 和 <H1>` 是一样的，但建议小写，因为大部分程序员都以小写为准。

2. 段落标签 `<p></p>`
    a. 段落标签 `<p>` 与换行 `<br>` 标签，使用上一点区别，`<p>` 是一对标签 `"<p></p>"`，而 `<br>` 是单独的标签。 
    b. 段落标签，每个段落之间有一定距离，类似于一个 `<p>` 换段标签等于使用两个 `<br>` 标签换行
    c. 常常使用段落标签，让文章条理段落上下分割清晰，同时也对搜索引擎优化（SEO），让搜索引擎感到你的网页内容段落
    清晰更加友好清晰。
    d. 浏览器会自动地在段落的前后添加空行。
    ps: 换行标签使用带关闭符 `<br />`;
    
3. html 属性
    a. 属性总是以名称/值对的形式出现，比如：`name="value"`。
    b. 属性值应该始终被包括在引号内，建议使用单引号 (值中有可能包含双引号)。 
    c. 属性值对建议统一使用小写。
    
4. `<h1 - h6> </h1 - h6>` 标题
    a. 正确使用标题标签 (搜索引擎使用标题为您的网页的结构和内容编制索引)
    b. 浏览器会自动地在标题的前后添加空行。
    c. 注意：因为 `h1`标签在网页中比较重要，所以一般h1标签被用在网站名称上。腾讯网站就是这样做的。如：`<h1> 腾讯网 </h1>`
    
5. `<hr />` 水平线
	`<hr />` 标签的在浏览器中的默认样式线条比较粗，颜色为灰色，可能有些人觉得这种样式不美观，没有关系，这些外在样式在我们以后学习了 css 样式表之后，都可以对其修改

6. `<!-- 这是一个注释 -->` 注释
    a. 开始括号之后（左边的括号）需要紧跟一个叹号，结束括号之前（右边的括号）不需要。
    ps: 当显示页面时，浏览器会移除源代码中多余的空格和空行。所有连续的空格或空行都会
    被算作一个空格。需要注意的是，HTML 代码中的所有连续的空行（换行）也被显示为一个空格。
    
7. `<a></a>` HTML 链接

    title 属性的作用，鼠标滑过链接文字时会显示这个属性的文本内容。这个属性在实际网页开发中作用很大，主要方便搜索引擎了解链接地址的内容（语义化更友好）。

    a. 链接一个网站 (原页面标签打开)
     `<a href = "http://www.baidu.com">baidu</a> `
    
    b. 在新标签打开网址链接
     `<a href = "http://www.baidu.com" target="_blank">baidu</a> `
     target = "_top" 则是跳出框架显示
    
    c. 定义页面跳转显示位置
     `<a href = "tips">tips</a>` 
     // 跳转到 tips 标签位置
     `<a href = "#tips">jump to tips</a> `
     // 跳转打其它页面 tips 位置
     `<a href = "http://www.baidu.com#tips">jump to tips</a>`
       
    d. 创建图片链接
     `<a href = "http://www.baidu.com#tips"><img src="xxx.gif" width="50" hight = "50">budi</a>`
    
    e. 创建邮件地址链接
     `<a href="mailto:someone@example.com?cc=someoneelse@example.com&bcc=andsomeoneelse
     @example.com&subject=Summer%20Party&body=You%20are%20invited%20to%20a%20big%20summer%20party!" 
     target="_top">Send mail!</a>`
     
     - 收件人：someone@example.com
     - 抄送：someoneelse@example.com
     - 秘密抄送：andsomeoneelse@example.com
     - 主题：Summer Party
     - 内容：You are invited to a big summer party! 
     
     注释：mailto：发送邮件地址；cc：抄送地址；bcc：秘密抄送地址；subject：标题；body：邮件内容；
     注意：？、& 的使用，空格需要用 %20 来代替。
       
    ps: 请始终将正斜杠添加到子文件夹。例如：`href="http://www.w3cschool.cc/html/`；
    如果写成：**`href="http://www.w3cschool.cc/html"`**，就会向服务器产生两次 HTTP 请求。
    **这是因为服务器会添加正斜杠到这个地址最后，然后再创建一个新的请求**。

8. `<head></head>` 元素

    a. 可以添加到头部元素标签
     `<title>, <style>, <meta>, <link>, <script>, <noscript>, and <base>`
    
    b. `<title></title>`
     必须元素；在浏览器工具栏，收藏夹，搜索引擎显示的标题。
    
    c. `<base>`
	`<base target="_blank" href="http://www.blog.luckykuku.com">`
	target:_self,_blank,_top,_parent
	- [详细用法可以参考](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/base)
	    
    d. `<link>` 
     定义了文档与外部资源之间的关系。
     通常用于链接样式表；例如：`<link rel="stylesheet" type="text/css" href="mystyle.css">`

    e. `<style></style>`
     定义样式渲染文档；如果是样式文件请使用 `<link>` 标签；
     例如：`<style type="text/css">body {background-color:yellow} p {color:blue}</style>`
        
    f. `<meta>`
     描述一些元数据，不显示，浏览器会解析；常用于指定网页的描述，关键词，文件的最后修改时间，作者，和其他元数据。
     元数据可以使用浏览器（如何显示内容或重新加载页面），搜索引擎（关键词），或其他Web服务。
     例如：
     
     - 为搜索引擎定义关键词
     `<meta name="keywords" content="HTML, CSS, XML, XHTML, JavaScript">`
     
     - 为网页定义描述内容
     `<meta name="description" content="Free Web tutorials on HTML and CSS">`
     
     - 定义网页作者
     `<meta name="author" content="Hege Refsnes">`
     
     - 每 30 秒后刷新当前页面
     `<meta http-equiv="refresh" content="30">`
     	- [详细用法参考](http://www.w3school.com.cn/tags/tag_meta.asp)

9.  `<table></table>` 表格
    - `<table>…</table>`：整个表格以 `<table>` 标记开始、`</table>` 标记结束。
    - `<caption>标题文本</caption>`: 用以描述表格内容，标题的显示位置：表格上方
    - `tbody>…</tbody>`：当表格内容非常多时，表格会下载一点显示一点，但如果加上 `<tbody>`标签后，这个表格就要等表格内容全部下载完才会显示。如右侧代码编辑器中的代码
   	- `<tr></tr>`: 表格行
   	- `<th></th>`: 表格表头,浏览器一般会用出题显示
    - `<td></td>`: 每行被分割的单元格数据(table data)
   	- 表格如果没有定义边框属性，表格是不现实边框。例如: `border="1"`
    - `<table summary="表格简介文本">`: 摘要的内容是不会在浏览器中显示出来的。它的作用是增加表格的可读性(语义化)，使搜索引擎更好的读懂表格内容，还可以使屏幕阅读器更好的帮助特殊用户读取表格内容.

10. `<div></div>` 和 `<span></span>` 区别
	- 块级元素在浏览器显示时，通常会以新行来开始（和结束）。主要用于文档布局。
	  实例: `<h1>, <p>, <ul>, <table>`
	
	- 内联元素在显示时通常不会以新行开始。提供了一种将文本的一部分或者文档的一部分独立出来的方式。
	实例: 
	  - `<b>, <td>, <a>, <img>`
	  - `<p>My mother has <span style="color:blue">blue</span> eyes.</p>`

11. `<!DOCTYPE html>` 标签
	- 作用：声明文档的解析类型(document.compatMode)，避免浏览器的怪异模式。
	`document.compatMode`：
		- BackCompat：怪异模式，浏览器使用自己的怪异模式解析渲染页面。
		- CSS1Compat：标准模式，浏览器使用W3C的标准解析渲染页面。
	- 参考:
		- [<!DOCTYPE html>很重要](http://angrycoder.iteye.com/blog/1757539)
		- [DOCTYPE声明作用及用法详解](http://www.jb51.net/web/34217.html)
		- [为什么使用<!DOCTYPE HTML>](https://i.wanz.im/2010/05/28/why_doctype_html/)

	- 注意：在 google 浏览器测试，如果 html 页面没有声明 `<!DOCTYPE html>` 和 写成 `<!DOCTYPE>`，使用 `document.compatMode` 得到的值都是 BackCompat(怪异模式)。

12. 强调语气
	`<em> 需要强调的文本 </em>`: 斜体
	`<strong> 需要强调的文本 </strong>`: 加粗
	差别:
	- `<em>` 和 `<strong>` 标签是为了强调一段话中的关键字时使用，它们的语义是强调。
	- `<span>` 标签是没有语义的，它的作用就是为了设置单独的样式用的

13. `<q><q/>`
	要引用的文本不用加双引号，浏览器会对 `<q>` 标签自动添加 **双引号**,
	注意这里用 `<q>` 标签的真正关键点不是它的默认样式双引号(如果这样我们不如自己在键盘上输入双引号就行了)，而是它的语义：**引用别人的话**。

14. 长文本引用
	`<blockquote>引用文本</blockquote>`
	浏览器对 `<blockquote>` 标签的解析是 **缩进** 样式，前后都有缩进

15. 为网页加入地址信息
	`<address>联系地址信息</address>`
	在浏览器上显示的样式为 **斜体**

16. 添加代码
	`<code>代码语言</code>`
	注：如果是多行代码，可以使用 `<pre>` 标签

17. `<pre>`
	标签不只是为显示计算机的源代码时用的，在你需要在网页中 **预显示格式** 时都可以使用它，只是 `<pre>` 标签的一个常见应用就是用来展示计算机的源代码

18. `<img>`
    `<img src="图片地址" alt="下载失败时的替换文本" title = "提示文本">`
    - src：标识图像的位置；
    - alt：指定图像的描述性文本，当图像不可见时（下载不成功时），可看到该属性指定的文本；
    - title：提供在图像可见时对图像的描述(鼠标滑过图片时显示的文本)；
    - 图像可以是 GIF，PNG，JPEG 格式的图像文件

19. `<form></form>`
    ```html
    <form method="post" action="http://a.test.com/kuku">
        <label for="username">用户名:</label>
        <input type="text" name="username" />
        <label for="pass">密码:</label>
        <input type="password" name="pass" />
        <input type="submit" value="Submit" />
        <input type="reset" value="重置">
    </form>
    ```
    - `<form>` ：`<form>` 标签是成对出现的，以 `<form>` 开始，以 `</form>` 结束。
    - action ：浏览者输入的数据被传送到的地方,比如一个PHP页面(save.php)。
    - method ： 数据传送的方式（get/post）

20. 文本输入框、密码输入框
    ```html
    <form>
        <input type="text/password" name="名称" value="文本" />
    </form>
    ```
    - type：
        - 当 type="text" 时，输入框为文本输入框;
        - 当 type="password"时, 输入框为密码输入框。
    - name：为文本框命名，以备后台程序 ASP 、PHP 使用。
    - value：为文本输入框设置默认值。(一般起到提示作用)

21. 文本域，支持多行文本输入
    ```html
    <form  method="post" action="http://a.test.com/kuku">
        <label>联系我们</label>
        <textarea cols="50" rows="10" >在这里输入内容...</textarea>
    </form>
    ```
    - `<textarea>` 标签是成对出现的，以 `<textarea>`开始，以 `</textarea>` 结束。
    - cols ：多行输入域的列数。
    - rows ：多行输入域的行数。
    - 在 `<textarea></textarea>` 标签之间可以输入默认值

22. 使用单选框、复选框，让用户选择
    ```html
    <form  method="post" action="http://a.test.com/kuku">
        <input   type="radio/checkbox"   value="值"    name="名称"   checked="checked"/>
    </form>
    ```
    - type:
        - 当 type="radio" 时，控件为单选框
        - 当 type="checkbox" 时，控件为复选框
    - value：提交数据到服务器的值（后台程序PHP使用）
    - name：为控件命名，以备后台程序 ASP、PHP 使用
    - checked：当设置 checked="checked" 时，该选项被默认选中
    - 注意:同一组的单选按钮，**name** 取值一定要一致，这样同一组的单选按钮才可以起到 **单选** 的作用

23. 下拉列表框
    ```html
    <form action="http://a.test.com/kuku" method="post" >
        <label>爱好:</label>
        <select>
          <option value="看书" selected="selected>看书</option>
          <option value="旅游">旅游</option>
          <option value="运动">运动</option>
          <option value="购物">购物</option>
        </select>
    </form>
    ```
    - selected="selected"：设置 selected="selected" 属性，则该选项就被默认选中
    - `<select>` 标签中设置 multiple="multiple" 属性，就可以实现多选功能，在 windows 操作系统下，进行多选时按下 Ctrl 键同时进行单击（在 Mac下使用 Command +单击），可以选择多个选项

24. `<label>`
    label 标签不会向用户呈现任何特殊效果，它的作用是为鼠标用户改进了可用性。如果你在 label 标签内点击文本，就会触发此控件。就是说，当用户单击选中该 label 标签时，浏览器就会自动将 **焦点 ** 转到和标签相关的表单控件上（就自动选中和该 label 标签相关连的表单控件上）。
    - `<label for="控件 id 名称">`
    注意：标签的 for 属性中的值应当与相关控件的 id 属性值一定要相同。
    ```html
    <form>
        <label for="male">男</label>
        <input type="radio" name="gender" id="male" />
        <br />
        <label for="female">女</label>
        <input type="radio" name="gender" id="female" />
        <br />
        <label for="email">输入你的邮箱地址</label>
        <input type="email" id="email" placeholder="Enter email">
    </form>
    ```


## 参考
- [HTML 标签-图文详解](http://www.cnblogs.com/smyhvae/p/4850684.html)
- [MDN HTML 元素参考](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element)
- [HTML 全局属性](http://www.w3school.com.cn/tags/html_ref_standardattributes.asp)
- [HTML 事件属性](http://www.w3school.com.cn/tags/html_ref_eventattributes.asp)
