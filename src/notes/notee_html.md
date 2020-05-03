# html

## Doctype 的作用？严格模式与混杂模式

Doctype 声明叫做文件类型定义（DTD），为了告诉浏览器该文件的类型，让浏览器知到该用哪个规范来解析文档

严格模式，又称标准模式，CSS1Compat

混杂模式，又称怪异模式或兼容模式，BackCompat

如何区分？

``` javascript
if(document.compatMode=="CSS1Compat" ) {
　　console.log('严格模式');
}else {
　　console.log('混杂模式');
}
```

区别

1. 盒模型，混杂的宽高包括 padding 与 border，标准不包括
2. 混杂可以设置行内元素宽高
3. div 中只有图片时，标准模式图片底部有 3 像素空白，怪异没有
4. 混杂模式中，表格中的字体不会继承他的祖先元素
5. 百分比宽高，标准模式由内容决定，混杂模式以父级元素的百分比设置
6. overflow 的溢出默认值，标准显示在包含元素外，混杂模式包含元素会自适应内容尺寸

意义

混杂模式服务于旧规则，严格模式服务于标准规则

## 行内元素有哪些？块级元素有哪些？空元素有哪些？

* 行内元素

a、b、span、img、input、strong、select、label、em、button、textarea

* 块级元素

div、ul、li、dl、dt、dd、p、h1-h6、blockquote

* 空元素

br、meta、hr、link、input、img