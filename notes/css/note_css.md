# css

#### 什么是盒子模型

CSS 技术所使用的一种思维模式：把网页中的元素都看成一个盒子，它具有：content，border，padding，margin 四个属性，即盒子模型

#### CSS 实现垂直水平居中

左右居中：

1. 元素设置了宽度的情况下，margin:  0 auto;
2. 将元素 display 设置为 inline-block，父元素 textaline: center;

上下左右居中：

1. position

``` 
父元素：
position:relative;
子元素：
position:absolute;
left:50%;
top:50%;
transform: translate(-50%,-50%);
```

2. flex 布局

``` 
父元素：
display: flex;//flex布局
justify-content: center;//使子项目水平居中
align-items: center;//使子项目垂直居中
```

#### src 与 href 的区别

href 是 Hypertext Reference 的缩写，表示超文本引用，用来建立当前元素和文档之间的连接，比如 a标签、link标签；文件加载在此处，下载目标文件的同时浏览器不会停止渲染

src 是 source 的缩写，即引入，src 指向的内容会嵌入到文档当前标签所在位置，比如 img、script；文件加载在此处，会暂停浏览器渲染直到该资源加载完毕

#### px 与 em 与 rem 的区别

px 表示绝对尺寸（实际为 css 中定义的像素），设置字体大小和元素宽高比较精确

em 表示相对尺寸，其相对于当前对象内文本的 font-size

rem 表示相对尺寸，其相对于根元素 <html> 的 font-size

#### 介绍 Flex 布局，flex 是什么属性的缩写

弹性盒布局，CSS3 的新属性，用于方便布局，比如垂直居中

flex属性是 flex-grow、flex-shrink 和 flex-basis 的简写

#### visibility 和 display 的差别（还有opacity)

visibility 设置 hidden 会隐藏元素，但是其位置还存在与页面文档流中，不会被删除，所以会触发浏览器渲染引擎的重绘

display 设置了 none 属性会隐藏元素，且其位置也不会被保留下来，所以会触发浏览器渲染引擎的回流和重绘。

opacity 会将元素设置为透明，但是其位置也在页面文档流中，不会被删除，所以会触发浏览器渲染引擎的重绘
