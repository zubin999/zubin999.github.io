---
title: leetcode:two-sum
date: 2021-10-01 12:26:10
tags: leetcode,php,tow sum
---

- 问题

> Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.
> 
> You may assume that each input would have exactly one solution, and you may not use the same element twice.
> 
> You can return the answer in any order.

- leetcode类别

简单


- 代码

```
class Solution {

    /**
     * @param Integer[] $nums
     * @param Integer $target
     * @return Integer[]
     */
    function twoSum($nums, $target) {
        $len = count($nums);

        for ($i = 0; $i < $len - 1; ++$i) {
            $sub = $target - $nums[$i];
            for ($j = $i + 1; $j < $len; ++$j) {
                if ($nums[$j] === $sub) {
                    return [$i, $j];
                }
            }
        }

        return [];
    }
}

```