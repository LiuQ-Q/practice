### 加载和执行（Loading and Exeution）

管理浏览器中的 JavaScript 代码是个很棘手的问题，因为代码执行过程会阻塞浏览器的其他进程，比如用户界面的绘制。每次遇到 `<script>` 标签，页面都必须停下来等待代码下载（如果是外链文件）并执行，然后继续处理其他部分。尽管如此，还是有几种方法能减少 JavaScript 对性能的影响：

* `</body>` 闭合标签之前，将所有的 `<script>` 标签放到页面底部。这能确保在脚本执行之前页面已经完成了渲染。
* 合并脚本。页面中的 `<script>` 标签越少，加载也就越快，响应也更迅速。无论外链文件还是内嵌脚本都是如此。
* 有多种阻塞下载 JavaScript 的方法：
    * 使用 `<script>` 标签的 defer 属性；
    * 使用动态创建的 `<script>` 元素来下载并执行代码；
    * 使用 XHR 对象下载 JavaScript 代码并注入页面中。

### 数据存取（Data Access）

### DOM 编程（DOM Scripting）

### 算法和流程控制（Algorithms and Flow Control）

### 字符串和正则表达式（Strings and Regular Expressions）

### 快速响应的用户界面（Responsive Interfaces）

### Ajax

### 编程实践（Programming Practices）