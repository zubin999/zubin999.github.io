---
title: 'leetcode: Median of Two Sorted Arrays'
date: 2021-11-03 09:53:45
tags: leetcode,php
---

- 问题

> Given two sorted arrays nums1 and nums2 of size m and n respectively, return the median of the two sorted arrays.
> 
> The overall run time complexity should be O(log (m+n)).



- 代码

```
class Solution {

    /**
     * @param Integer[] $nums1
     * @param Integer[] $nums2
     * @return Float
     */
    function findMedianSortedArrays($nums1, $nums2) {
        $nums = array_merge($nums1, $nums2);
        $len = count($nums);
        $nums = $this->bubbleSort($nums, $len);

        if ($len % 2) {
            return $nums[floor($len / 2)];
        }

        return ($nums[$len / 2] + $nums[$len / 2 - 1]) / 2;
    }

    function bubbleSort($arr, $n)
    {
        for($i = 0; $i < $n; $i++)
        {
            $swapped = false;
            for ($j = 0; $j < $n - $i - 1; $j++)
            {
                if ($arr[$j] > $arr[$j+1])
                {
                    $t = $arr[$j];
                    $arr[$j] = $arr[$j+1];
                    $arr[$j+1] = $t;
                    $swapped = true;
                }
            }

            if($swapped === false) {
                break;
            }
        }

        return $arr;
    }
}
```