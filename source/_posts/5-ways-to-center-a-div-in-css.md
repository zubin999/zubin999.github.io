---
title: css中5种居中方式
date: 2022-10-11 09:33:13
tags: css,居中
---

![](/2022/10/11/5-ways-to-center-a-div-in-css/1.png)

如上图,没有给任何位置设置.

```
// css
<style>
    .big {
        width: 400px;
        height: 300px;
        background-color: #1edcff;
    }

    .small {
        width: 70px;
        height: 70px;
        background-color: #3ca217;
    }
</style>

// html
<div class="big">
    <div class="small"></div>
</div>
```
要是小的div在大div里居中,有5种方式.

1. flex

```
.big {
    width: 400px;
    height: 300px;
    background-color: #1edcff;

    display: flex;
    align-items: center;
    justify-content: center;
}
```

2. grid


```
.big {
    width: 400px;
    height: 300px;
    background-color: #1edcff;

    display: grid;
    place-content: center;
}

```

3. position


```
.big {
    width: 400px;
    height: 300px;
    background-color: #1edcff;

    position: relative;
}

.small {
    width: 70px;
    height: 70px;
    background-color: #3ca217;

    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```

4. flex + margin


```
.big {
    width: 400px;
    height: 300px;
    background-color: #1edcff;

    display: flex;
}

.small {
    width: 70px;
    height: 70px;
    background-color: #3ca217;

    margin: auto;
}
```

5. grid + margin

```
.big {
    width: 400px;
    height: 300px;
    background-color: #1edcff;

    display: grid;
}

.small {
    width: 70px;
    height: 70px;
    background-color: #3ca217;

    margin: auto;
}
```

以上任意一种,都可以看到下面居中效果:

![](/2022/10/11/5-ways-to-center-a-div-in-css/2.png)