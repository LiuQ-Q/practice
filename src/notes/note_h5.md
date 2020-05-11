# H5笔记

##  html5 概述

​	HTML5 是定义 HTML 标准的最新版本。该术语表示两个不同的概念：

​		它是一个新版本的 HTML 语言，具有新的元素，属性和行为

​		它有更大的技术集，允许更多样化和强大的网站和应用程序

​		HTML5 约等于 HTML + CSS + JS

### HTML5 优势

​	跨平台：唯一一个通吃Windows MAC Iphone Android 等主流平台的跨平台语言

​	快速迭代

​	降低成本

​	导流入口多

​	分发效率高

### 渲染模式与 <DOCTYPE  html> 的作用

​	浏览器三种渲染模式

​		standards mode

​		almost standards mode

​		quirks mode

​	告诉浏览器以什么模式渲染，加 DOCTYPE 为标准模式，不加为怪异模式

### 获取当前模式

``` 
document.compatMode
```

​	在 ie9 往上的浏览器，三种模式在渲染方面几乎没有区别

​	在 ie 7 8 9 中，理论上存在怪异模式，实际没有怪异模式

​	在 ie6 中，标准模式和怪异模式渲染区别最大

​	在 ie6 以下浏览器，只有怪异模式

## H4 与 H5 的区别

### 标签

* DOCTYPE 标签

  H5 不需要引用 dtd，dtd 已过时

* 根元素

  H4 根元素：

``` 
<html xmlns="http://www.w3.org/1999/xhtml"></html>
```

​		标记没有问题，可继续用，在 H5 中可省略

​		xmlns 是 HTML 1.0 中的东西，意思是这个页面中的元素都位于此命名空间中

* head 元素

  MIME 类型：

  ​	比如 Content-Type:text / html，text / html 即为这个页面的“内容类型”或称为 MIME 类型

  H5 中没有此属性，因为已经被移植到 http 协议中

## 语义化标签

​	在 H5 出来之前，我们用 div 来表示页面的头部，章节，页脚等，但这些 div 没有实际意义，经统计上百万页面，div 的 id 或 class 大量重复，所以 H5 元素引入了语义化标签

```
<header></header>
<nav></nav>
<aside></aside>
<article></article>
<section></section>
<footer></footer>
```

语义化的好处：

HTML 结构清晰 , 代码可读性好 , 方便维护
有利于 SEO
浏览器对语义化标签有默认样式 , 保证了基本的结构 , 没有 css 也不会乱掉
方便其他设备解析 , 如屏幕阅读器

## h5 中移出的标签元素

```
<acronym>
<applet>
<basefont>
<big>
<center>
<dir>
<font>
<frame>
<frameset>
<noframes>
<strike>
<tt>
```

## property 与 attribute

### 1. 什么是 attribute， 什么是 property

​	html 标签的预定义属性和自定义属性我们统称为 attribute

​	js 原生对象的直接属性，我们统称为 property

### 2. 什么是布尔值属性，什么是非布尔值属性

​	property 的属性值为布尔类型，我们统称为布尔值属性

​	property 的属性值为非。。。，。。。非。。。

### 3. attribute 和 property 的同步关系 

​	非布尔值属性：实时同步

​	布尔值属性：

​		property 永远不会同步 attribute

​		在没有动过 property 的情况下，attribute 会同步 property

​		在动过 property 的情况下，attribute 不会同步 property

### 4. 用户操作的是 property，浏览器认的是 property

## h5 中的小功能

添加、删除、切换 class

``````
var testNode = document.querySelector("#test")
testNode.classList.add("aaa")
testNode.classList.remove("aaa")
testNode.classList.toggle("aaa")
``````



data-###

获取或修改标签中以 data- 开头的属性，获取时去掉 - 并改为驼峰命名

``` 
<div id="test" data-lq-aaa="aaa"></div>

var testNode = document.querySelector("#test")
console.log(testNode.dataset.lqAaa)
testNode.dataset.lqAaa="bbb"
```



contenteditable

其中内容是否可编辑

``` 
<div id="test" contenteditable="true">
	adsfasfadsadsf
</div>
```

### html5 新特性

1. 语义化标签

   header , footer , aside , section , article , nav

2. 表单输入类型

   email , url , number , range , Date Pickers , search , color

3. 表单属性

   autocomplete , placeholder , form

4. 视频音频

   video , audio

5. 画布

   canvas

6. 伸缩矢量图

   svg

7. 拖拽属性

   draggable

8. 事件

   resize , reput

9. 地理定位

   Geolocation

10. web存储

    sessionStorage , localStorage

11. 应用程序缓存 application cache

    创建 cache mainifest 文件

12. 文件通讯协议

    websocket

13. 文件读取

    fileReader

14. 类名操作

    classlist
