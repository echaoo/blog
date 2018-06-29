---
title: fetch设置header.Authorization成功，单请求头中没有的问题
date: 2018-06-29
tags: 
---
##### 问题描述
最近在项目开发中遇到了一个"奇怪"的问题：

项目中想要加一层身份验证，需要设置header.Authorization，并使用fetch发送一个请求，代码如下：
```js
const newOptions = {
  headers: {
     Accept: 'application/json',
     'Content-Type': 'application/json; charset=utf-8',
     Authorization: Authorization
  }
}
fetch(url, newOptions)
  .then(response => {   return response.json();})
  .catch(e => {console.log(e)})
```

使用postman调试接口是正常的，但是使用上面的代码发请求时，就会报401错误，request header里并没有我设置的Authorization，并且请求方式为OPTIONS（忘了留截图），emmmm，这是为什么呢？


##### 原因
首先，OPTIONS用来请求时的预检，用以判断实际发送的请求是否安全，那么问题来了，什么时候请求时需要预检呢？

参考资料：https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS

浏览器由于安全原因，做了跨域请求限制。跨域资源共享（ CORS ）机制允许 Web 应用服务器进行跨域访问控制，从而使跨域数据传输得以安全进行。浏览器支持在 API 容器中（例如 XMLHttpRequest 或 Fetch ）使用 CORS，以降低跨域 HTTP 请求所带来的风险。


**跨域资源共享标准新增了一组 HTTP 首部字段，允许服务器声明哪些源站有权限访问哪些资源。另外，规范要求，对那些可能对服务器数据产生副作用的 HTTP 请求方法（特别是 GET 以外的 HTTP 请求，或者搭配某些 MIME 类型的 POST 请求），浏览器必须首先使用 OPTIONS 方法发起一个预检请求（preflight request），从而获知服务端是否允许该跨域请求。服务器确认允许之后，才发起实际的 HTTP 请求。在预检请求的返回中，服务器端也可以通知客户端，是否需要携带身份凭证（包括 Cookies 和 HTTP 认证相关数据）。**


##### 解决方法

后端接口需要允许这个OPTIONS请求通过,通过后才会有下一步的请求。会发现前端同一个请求发送了两次, 在第二条请求里你可以发现你的Authorization已经显示你面前了。