---
title: JavaScript中innerText和textContent的区别
date: 2015-07-03 20:36:42
tags: JavaScript,innerText,textContent
---

`innerText`和`textContent`两个属性都用于得到文本文字，一直以来以为他俩没有分别，可是今天在本地做好的项目，部署到服务器后，我用Firefox浏览时，发现出错了，而控制台的错误是undefined，这就让我纳闷了。于是，通过google一番，才知道原来这两个还是有分别的。主要有3点，这里有对<a title="Node.textContent" href="https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent" target="_blank">textContent</a>的详细介绍。这里只说下不同。

<p>Internet Explore引入了element.innerText,用途大体相同但是一下几个不同点：</p>

- textContent能得到所有元素内容，包括script和style元素。但是，IE独有属性innerText不可以。
- innerText受style影响不会返回隐藏的文本，而textContent不会
- 由于innerText受CSS样式影响，会产生一个回流，而textContent不会

另外，还需注意的是，在stackoverflow上有人提到，虽然innerText是IE独有属性，主流浏览器都支持，除了Firefox，而IE也是唯一一个不支持textContent的浏览器。这就解释了为什么我的项目在本地正常，而到了服务器就出问题了。但是，我们不能要求用户去使用某一种浏览器，那么该如何解决呢？

- 使用jquery是最好的，最简便的。
- 做一个兼容判断，代码如下：

``` JavaScript
function getText(elem){
     if(typeof elem.textContent != 'undefined'){
        return elem.textContent;
    }else{
        return elem.innerText;
    }
}
```