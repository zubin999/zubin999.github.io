---
title: leetcode:Container With Most Water
date: 2022-03-09 00:20:56
tags: leetcode,php
---

- 问题

> Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of the line i is at (i, ai) and (i, 0). Find two lines, which, together with the x-axis forms a container, such that the container contains the most water.

这里有一个数组,长度在2~10^5之间.求这些数字在坐标中组成的最大的长方形面积.


- 解决

最容易想到的是直接遍历:

```
class Solution {


    /**
     * @param Integer[] $height
     * @return Integer
     */
    function maxArea($height) {
        $len = count($height);
        $area = 0;
        for($i = 0, $len1 = $len - 1; $i < $len1; $i++) {
             for($j = $len; $j > $i; $j--) {
                 $area = max(min($height[$i], $height[$j]) * ($j - $i), $area);
             }
        }
        
        return $area;
    }
}
```

我开始想的还多一步,定义了一个数组将外层循环每一轮最大值存下来最后再求最大值,对于数组长度不大时没有问题,但是题目中数组长度能够达到10^5,这段代码提交的结果是超时.

后来看他们的解决方法,才发现其实也很简单.

```
class Solution {


    /**
     * @param Integer[] $height
     * @return Integer
     */
    function maxArea($height) {
        $len = count($height);
        $area = 0;
        $l = 0;
        $r = $len - 1;
        while($l < $r) {
            $area = max($area, min($height[$l], $height[$r]) * ($r - $l));
            if($height[$l] < $height[$r]) {
                $l++;
            } else {
                $r--;
            }
        }

        return $area;
    }
}
```

首先求最左边和最右边的结果,因为这时长度最长.然后看左边和右边哪个数小,相应的就去找下一个值.
