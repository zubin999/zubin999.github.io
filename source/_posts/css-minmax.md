---
title: CSS minmax()
date: 2022-10-05 13:46:30
tags: CSS,minmax
---

- 认识minmax


看看[`MDN`](https://developer.mozilla.org/en-US/docs/Web/CSS/minmax)上对`minmax`的定义:

> The minmax() CSS function defines a size range greater than or equal to min and less than or equal to max. It is used with CSS Grids

可以得出3点:

1. `minmax`是`CSS`中的一个函数
2. 定义了一个范围:大于等于最小值并且小于等于最大值
3. 在CSS的grid中使用

现在来使用它:

``` css
<style>
    .box {
        display: grid;
        grid-template-columns: minmax(200px, 500px) 1fr 1fr;
        grid-gap: 10px;
    }

    .box div {
        height: 78px;
        background-color: #c2c;
    }
</style>
```

``` html
<div class="box">
    <div>A</div>
    <div>B</div>
    <div>C</div>
    <div>D</div>
    <div>E</div>
    <div>F</div>
    <div>G</div>
    <div>H</div>
    <div>I</div>
</div>
```

在页面中可以看到:

![](/2022/10/05/css-minmax/0.png)

此时,第1列的宽度为500px,缩小页面宽度,第2,3列的宽度在变化,第1列的宽度没有改变.当第2,3列的宽度不能再小后,第1列的宽度开始变小,直到小到200px后,不再改变.

![](/2022/10/05/css-minmax/1.png)
![](/2022/10/05/css-minmax/2.png)

那么,上面定义得出的第1,2点都得到了证明.第3点,试下对box里面的div的高度使用`minmax`函数,看是否生效.

``` css
.box div {
    height: minmax(78px, 120px);
    background-color: #c2c;
}
```

![](/2022/10/05/css-minmax/3.png)

不论页面高度怎么变化,每个`div`的高度都没有改变.说明`minmax`确实不能在其他地方使用.

- 注意

1. `minmax(min, max)`的最小值(第一个参数)不能大于最大值(第二个参数).如: 将上面的改为: `minmax(300px, 200px)`,那么`200px`无效,只有300px有效,第1列固定为300px.

![](/2022/10/05/css-minmax/4.png)

1. `minmax`的最小值不能设置为`1fr`,最大值可以.
