---
title: 如何给JavaScript中函数的参数添加默认值
date: 2015-05-10 08:10:48
tags: JavaScript,js,函数,默认值
---

前段时间在写js代码时,有个地方需要给函数的参数赋默认值.于是,我毫不犹豫地在参数后面直接给了值,如:

``` JavaScript
function sum(a,b=2) {}
```

`*ES6已支持上面的语法`

因为平时写PHP代码时都是这样,说实话以前还真没有给js参数赋过默认值,但是一写出来就报错了.不用说,肯定是js不能这样使.于是上网查找了 一番,终于明白在js里给参数默认值,可以这样:

``` JavaScript
function sum(a){
    var b = arguments[1]?arguments[1]:2;
}
```

马上写了个测试试试,还真可以.为什么可以这样写呢?原因是js在编译时会把参数放入arguments这个数组里面,这让我想起了PHP里的`func_get_args()`.但是,在一想,有问题啊!

是什么问题呢?js里面在作真假判断时,遇上`undefined`,`null`,`0`,`''`等这些情况时,是会返回假的,那么在这里,如果我调用这个函数,给b的值为零的话,那它不是会用默认值?情况真的是这样吗?试一试不就知道.

``` JavaScript
function sum(a){

var b = arguments[1]?arguments[1]:2;

alert(a+b);

}

sum(1,0);
```

结果,得到的还真是3,看来在使用的时候还需要注意一下啊.
