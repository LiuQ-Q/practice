# Http

#### 浏览器发出一个请求是怎样的过程?
1. 输入网址
2. 缓存解析
3. DNS解析
4. tcp连接,三次握手
5. 页面渲染
#### tcp三次握手,四次挥手

<a href="https://blog.csdn.net/qq_38950316/article/details/81087809" target="_blank">TCP的三次握手与四次挥手理解及面试题</a>

#### localStorage与sessionStorage区别
localStorage与sessionStorage都是用来存储客户端临时信息的对象
localStorage的生命周期是永久,除非主动清除
sessionStroage的生命周期为当前窗口或标签页
不同浏览器无法共享localStorage和sessionStorage中的信息
相同浏览器的同源不同页面共享localStorage,不同页面无法共享sessionStorage
如果一个页面包含多个iframe标签,他们共享sessionStorage中的信息