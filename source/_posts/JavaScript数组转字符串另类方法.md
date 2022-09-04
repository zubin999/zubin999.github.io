---
title: JavaScript数组转字符串另类方法
date: 2015-12-07 22:19:17
tags: JavaScript,数组,字符串
---

通常，当我们想将一个数组，如*['hello','world']*转换为字符串的话，会使用join方法将其连接为一个字符串。前两天在看书的时候，学到了新的方法,虽然不是什么高级技巧，但是，也在此记录一下，说明自己以前没有去思考过。

 - 方法一,使用toString方法：

<pre lang="javascript">
var a = ['hello','world'];
console.log(a.toString()); //hello,word
</pre>

 - 方法二,使用强制类型转换：

<pre lang='javascript'>
var a = ['hello','world'];
console.log(String(a));
</pre>

但是，需要注意的是，这里的两个方法只能以逗号将其连接，如果要以其他的方式，还是需要使用join方法。