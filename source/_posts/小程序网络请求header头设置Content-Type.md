---
title: 小程序网络请求header头设置Content-Type
date: 2019-08-16 06:46:04
tags: 小程序,header
---

小程序发起`http`网络请求，通过`wx.request`函数的调用。该函数接收一个对象，可以在对象里配置包括`地址`、`header`、`method`等。`method`默认为`post`请求。但是，我们在服务器端使用`$_POST`或框架如Phalcon里的`$this->request->getPost()`等获取参数时，取得结果为空。但是通过流获取:

```
$params = file_get_contents('php://input')
```



`Phalcon`里通过：

```
$params = $this->request->getJsonRawBody();
```

这样是能够得到参数的。为什么`POST`请求，却不能在`POST`里得到呢？我们注意看文档，会发现此时的`header`里`content-type`为`application/json`(默认方式)，再查看PHP的[文档](https://www.php.net/manual/zh/reserved.variables.post.php)，可以看到：

> 当 HTTP POST 请求的 Content-Type 是 application/x-www-form-urlencoded 或 multipart/form-data 时，会将变量以关联数组形式传入当前脚本。

也就是说，我们需要重新设置小程序的`header`头里的`content-type`为`application/x-www-form-urlencoded`，就可以获取`POST`里的参数了。

