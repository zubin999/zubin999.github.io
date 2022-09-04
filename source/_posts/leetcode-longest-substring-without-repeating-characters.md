---
title: 'leetcode: longest substring without repeating characters'
date: 2021-10-09 17:29:18
tags: leetcode,php
---

- 问题

> Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.
> 
> You may assume that each input would have exactly one solution, and you may not use the same element twice.
> 
> You can return the answer in any order.

- leetcode类别

中等

- 代码

```
class Solution {

    /**
     * @param String $s
     * @return Integer
     */
    function lengthOfLongestSubstring($s) {
        $len1 = strlen($s);
        if (!$len1) {
            return $len1;
        }

        $max = 1;
        $p = [];
        for($i = 0, $j = 0; $i < $len1; $i++) {
            $code = ord($s[$i]);
            if (!array_key_exists($code, $p)) {
                $p[$code] = $i;
                continue;
            }
            
            $strlen = $i - $j;
            $max = $strlen > $max ? $strlen : $max;
            $i = $p[$code];
            $j = $i + 1;
            $p = [];
        }
        
        $cnt = count($p);
        $max = $cnt > $max ? $cnt : $max;
        
        return $max;
    }
}
```