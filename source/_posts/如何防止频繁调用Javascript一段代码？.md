---
title: 如何防止频繁调用Javascript一段代码？
date: 2016-08-08 23:07:14
tags: javascript,频繁调用
---

**应用场景**
首先说下什么时候会用到，当我们给页面注册了`scroll`,`resize`等这类事件时，可能就需要用到了。为什么呢？比如，我们需要做一个页面滚动条滚动到底部时就自动加载下一页的功能，那么当滚动条滚动时就会频繁的触发`scroll`事件。为了减少js的执行，提升页面的性能，我们就需要滚动条在页面底部时才触发加载。

**代码实现**
```
function debounce(func, wait, immediate) {
        var timeout;
        return function() {
            var context = this, args = arguments;
            var later = function() {
                timeout = null;
                if (!immediate) func.apply(context, args);
            };
            var callNow = immediate && !timeout;
            clearTimeout(timeout);
            timeout = setTimeout(later, wait);
            if (callNow) func.apply(context, args);
        };
    }

```