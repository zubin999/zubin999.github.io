---
title: JavaScript参数默认值(ES6新方法)
date: 2015-07-25 02:45:29
tags: JavaScript,js,es6,默认值
---

在之前，我有写过两篇关于JavaScript参数赋默认值和变量默认值的文章，今天又来谈谈JavaScript的默认参数。你不要问我，就一个默认参数还有什么好说的？我们都知道，JavaScript以ECMAScript为标准，而在今年6月17号，ECMAScript发布了新的版本，官方叫作ECMAScript 2015，但通常大家称其为ECMAScript 6或者 ES6。而我今天要说的就是这个新版本里的默认参数。在ES 6里，函数可以初始化参数附带的默认值，如果这个参数没有传值或传入一个undefined。简单的说就和很多其他语言一样可以这样写：

<pre lang="javascript">
function  defaultValue(a,b=3){
    return a+b;
}

var val1 = defaultValue(3);   //val=6
var val2 = defaultValue(3,undefined);  //val = 6
</pre>

在以前，这样写就是错误的，那么现在，当你在调用这个函数时，只传值给a，或者b传入undefined，b的值都为3。说到以前，那我在[如何给JavaScript参数赋默认值][1]中有说到利用arguments对象，这种方法在定义函数时，只需声明必填参数即可。还有一种方法是利用typeof作判断来赋默认值，如下：

<code lang="javascript">
function typeofSetDefaultValue(a,b){
    b = typeof b !== 'undefined' ? b : 1;
    return a+b;
}
</code>

相对于这两种方法，ES 6的新特性确实带来极大方便。但是，缺点就是，现在的主流浏览器还不支持。嘿，浏览器虽然暂时还不支持，也别表现出悲观，来点更兴奋的！你可以将一个函数当作默认值赋值给另一个函数的参数，如：

<code lang="javascript">
function funcToBeDefaultValue(a = surprise()){
    return a;
}
function surprise(){
    return 'surprise';
}
</code>

感觉被玩坏了？还有更会玩的。你还可以将它的邻居赋值给它：

<code lang="javascript">
function laterToDefaultValue(a,b = a + ' ',c = b + "world"){
    return c;
}
var str = laterToDefaultValue('hello');  //str = "hello world";
</code>

更多关于JavaScript默认值信息请看这里：

 1. [Default parameters][2]
 2. [parameter_default_value][3]


  [1]: /2015/05/10/%E5%A6%82%E4%BD%95%E7%BB%99JavaScript%E4%B8%AD%E5%87%BD%E6%95%B0%E7%9A%84%E5%8F%82%E6%95%B0%E6%B7%BB%E5%8A%A0%E9%BB%98%E8%AE%A4%E5%80%BC/
  [2]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters
  [3]: http://wiki.ecmascript.org/doku.php?id=harmony:parameter_default_values
