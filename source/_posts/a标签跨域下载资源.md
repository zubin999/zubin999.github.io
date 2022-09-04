---
title: a标签跨域下载资源
date: 2022-02-25 20:27:50
tags:
---

- 前言

下载功能在网站中十分常见,实现的方式也多样,可以通过后端也可以通过前端.比如:`a`标签.

- 问题

`a`标签有一个属性:`download`.`a标签`大多数时候都是用于跳转一个链接.但是加上`download`属性之后便不会再跳转该链接,而是成了下载.但是这里的下载是有[限制条件][1]的.这里清楚的说到:

> `download`属性仅对同源地址或者`blob`和`data`协议的有效.

也就是说甲网站想要在网页里用a标签直接下载乙网站的视频或网页是不行的,因为他们域名不同.即使加上`download`属性,也无效,浏览器也会忽略.

- 解决方案

虽然不能直接下载,但是后面有提到,`blob`,`data`这类是不受限制的,例:

```
var x = new XMLHttpRequest();
x.open("GET", link, true);
x.responseType = 'blob';
x.onload=(e)=> {
    var url = window.URL.createObjectURL(x.response)
    var a = document.createElement('a')
    a.href = url
    a.download = fileName
    a.click()
}
x.send();
```

这里是通过`ajax`发起一个`get`请求获取到要下载的资源,返回二进制流数据.得到数据后在用`a标签`去下载


  [1]: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a