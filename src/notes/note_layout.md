# 布局

#### 静态布局

Static Layout , 传统 web 页面布局 , 常用于 PC 端 , 一般高度和宽度固定 ，超出浏览器的部分使用滚动条查阅
**优势:** 设计简单 , 没有浏览器兼容性问题
**劣势:** 不能适配不同尺寸屏幕

#### 流式布局

Liquid Layout , 页面元素以百分比设置属性 , 元素宽高能根据分辨率调整 , 整体布局不变
**优势:** 可适应不用尺寸屏幕
**劣势:** 屏幕尺寸跨度过大时 , 一些元素会显得不协调

#### 自适应布局

Adaptive Layout , 即创建多个静态布局 , 使用 **媒体查询** 根据不同分辨率切换静态布局
**优势:** 适配不同尺寸屏幕
**劣势:** 需要开发多套页面 , 费时费力

#### 响应式布局

Responsive Layout , 使用 **媒体查询** 监测设备屏幕大小 , 有针对性的更改页面布局
**优势:** 一个页面在所有终端上都有令人满意的效果
**劣势:** 移动端与 pc 端加载同样内容 , 虽然元素隐藏 , 但文件大加载会慢 ; 不适合一下大型内容过多的网站

#### rem/em 布局

em 相对于父元素 , rem 相对于 html 标签 , 
**优势:** 改变根元素 font-size 即可改变页面大部分元素尺寸
**劣势:** 每个数值都需要计算

#### flex 布局

Flexible Box 弹性布局

**优势:** 容易上手 , 容易达到布局效果
**劣势:** ie9 以上才支持