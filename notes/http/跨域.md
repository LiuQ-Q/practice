# 跨域解决方案

## JSONP 跨域

jsonp 的原理就是利用 script 标签没有跨域限制，通过 script 标签 src 属性，发送带有 callback 参数的 GET 请求，服务端将接口返回数据放到 callback 函数中，返回给浏览器，浏览器解析执行，从而前端拿到 callback 函数中的数据。

缺点：只能发送 get 请求

## document.domain + iframe 跨域

此方案仅限主域相同，子域不同的跨域应用场景。实现原理：两个页面都通过js强制设置document.domain为基础主域，就实现了同域。

## location.hash + iframe 跨域

## window.name + iframe 跨域

window的name属性有个特征：在一个窗口(window)的生命周期内,窗口载入的所有的页面都是共享一个window.name的，每个页面对window.name都有读写的权限，window.name是持久存在一个窗口载入过的所有页面中的，并不会因新页面的载入而进行重置。

## postMessage 跨域

window.postMessages是html5中实现跨域访问的一种新方式，可以使用它来向其它的window对象发送消息，无论这个window对象是属于同源或不同源。

## WebSocket 协议跨域

## 代理跨域

nginx 代理跨域
node 中间件代理跨域

## 跨域资源共享 (CORS)

CORS是一个W3C标准，全称是"跨域资源共享"（Cross-origin resource sharing）。
它允许浏览器向跨源服务器，发出XMLHttpRequest请求，从而克服了AJAX只能同源使用的限制。
CORS需要浏览器和服务器同时支持。目前，所有浏览器都支持该功能，IE浏览器不能低于IE10。

#### 浏览器会将ajax请求分为两类，其处理方案略有差异：简单请求、特殊请求。

#### 简单请求

只要同时满足一下两大条件

请求方法是一下三种方法之一：

* HEAD
* GET
* POST

HTTP 的头信息不超出以下几种字段：

* Accept
* Accept-Language
* Content-Language
* Last-Event-ID
* Content-Type: 只限于三个值 
` application/x-www-form-urlencoded `
` mutipart/form-data `
` text/plain `

当浏览器发现 ajax 请求是简单请求时，会在请求头(Request Headers)中携带一个字段： ` Origin `
Origin 中会指出当前请求属于哪个域，服务器会根据这个值决定是否允许其跨域。

如果服务器允许跨域，会在响应头中携带下面的信息：

```
// 可接受的域，是一个具体的域名或者 *
Access-Control-Allow-Oringin: *
// 是否允许携带 cookie，默认 cros 不携带 cookie，除非这个值时 true
Access-Control-Allow-Credentials: true
Content-Type: text-html; charset=utf-8
```

如果跨域请求想操作 cookie，需要满足三个条件：
* 响应头中携带 Access-Control-Allow-Credentials: true
* 浏览器发送 ajax 需要指定 withCredentials: true
* 响应头中的 Access-Control-Allow-Origin 不能为 *

#### 复杂请求

复杂请求会在正式通信前，增加一次 HTTP 查询请求，成为预检请求(preflight)。
浏览器会先询问服务器，当前网页所在域名是否在服务器的许可名单中，以及可以使用哪些 HTTP 动词和头信息字段，只有得到肯定答复，浏览器才会发出正式的 ajax 请求，否则报错

预检请求中，除了 Origin 之外，多了 2 个头：

* Access-Control-Request-Method: 接下来会用到的请求方式，比如PUT
* Access-Control-Request-Headers: 会额外用到的头信息

服务器收到请求，如果许可跨域，会发出响应，除了 Access-Control-Allow-Origin 和 Access-Control-Allow-Credentials 以外，这里又额外多出3个头：

* Access-Control-Allow-Methods: 允许访问的方式
* Access-Control-Allow-Headers: 允许携带的头
* Access-Control-Max-Age: 本次许可的有效时长，单位是秒，过期之前的ajax请求就无需再次进行预检了