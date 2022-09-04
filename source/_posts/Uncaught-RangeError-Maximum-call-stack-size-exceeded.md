---
title: Uncaught RangeError:Maximum call stack size exceeded
date: 2016-07-12 21:56:59
tags: ajax
---

在做`ajax`交互时，出现了点击按钮毫无反应，初初以为是事件绑定问题，检查过代码过后确认应该是其他问题。后来`chrome`的`console`控制台提示：
```
Uncaught RangeError:Maximum call stack size exceeded
```

于是，使用firefox developer版调试，`console`控制台报错：

```
TypeError: 'stepUp' called on an object that does not implement interface HTMLInputElement
```

终于找到了一个比较重要的错误提示，于是上Google搜索，在`stackoverflow`上找到问题，是因为`$.ajax`的`data`属性里加入了`html`元素，即：

```
data = $('[name=name]').val()
```

后面的`val()`没有写导致的。