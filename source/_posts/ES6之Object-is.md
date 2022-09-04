---
title: ES6之Object.is
date: 2015-08-02 23:54:22
tags: es6,javascript
---

当你想比较两个值是否相等时，你可能会用相等运算符“==”或“===”，这两者的区别在于后者会增加类型的比较，虽然这样，有时依然不能够准确的比较.+0 === -0 返回true, 另外，NaN === NaN 得到的是false,这需要使用isNaN来检测NaN属性。

ECMAScript6引入了Object.is()来弥补遗留的这个问题。这个方法接收两个参数并且如果值相等返回true。两个参数在类型和值一样时会被判为相等。大多数情况下，Object.is()和"==="是一样的，不同的是使用Object.is()对+0和-0比较，返回false，而NaN返回true。下面是一些例子：

<pre lang="javascript">
console.log(+0 == -0);              // true
console.log(+0 === -0);             // true
console.log(Object.is(+0, -0));     // false

console.log(NaN == NaN);            // false
console.log(NaN === NaN);           // false
console.log(Object.is(NaN, NaN));   // true

console.log(5 == 5);                // true
console.log(5 == "5");              // true
console.log(5 === 5);               // true
console.log(5 === "5");             // false
console.log(Object.is(5, 5));       // true
console.log(Object.is(5, "5"));     // false
</pre>