---
title: 'Uncaught TypeError: window.opener is null'
date: 2022-01-12 16:33:18
tags: javascript,window.opener
---

HTML中a标签可以设置target属性,其属性值包括:

- _self 默认值
- _blank
- _parent
- _top

其中,_blank会打开一个新的页卡或窗口,A页面通过a标签打开了B页面:

```
<a href="http://example.com" target="_blank">Go</a>
```

在B页面中想对A页面操作,那么可以使用window.opener.
可是,如果通过a标签_blank打开的窗口,默认会有一个rel属性并且值为noopener.这就造成B页面中出错:

```
Uncaught TypeError: window.opener is null
```

解决这个问题,我们只需在A页面的a标签上加上rel属性,并且值等于opener就解决了.

```
<a href="http://example.com" target="_blank" rel="opener">Go</a>
```

对此在[https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a][1]中有明确的详细的说明.

如果在A页面中打开B页面是通过js来实现:

```
window.open("http://example.com", '_blank');
```

这样,是不会存在a标签那样的问题


  [1]: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a