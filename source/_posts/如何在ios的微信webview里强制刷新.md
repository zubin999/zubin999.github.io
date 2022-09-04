---
title: 如何在ios的微信webview里强制刷新
date: 2017-07-19 15:13:28
tags: ios,webview,强制刷新
---

移动端网页webview中遇到了一个问题,A页面进入B页面做了一些操作,影响了A页面部分数据，再返回到A页面,受影响数据应该显示新数据。在android的手机里面使用自带虚拟返回按钮，不会存在这样的问题。但是在ios里面，使用自带返回按钮回到的界面并没有更新。这是为什么？原因是**`bfcache`**(**`back-forward cache`**)。[详细描述看这里][1]

解决方案链接的文章里也有说道，我这里采用的是监听pageonshow事件:

``` javascript
window.onpageshow = function(event) {
      if (event.persisted) {
            window.location.reload()
      }
};
```


  [1]: https://developer.mozilla.org/en-US/Firefox/Releases/1.5/Using_Firefox_1.5_caching