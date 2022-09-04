---
title: 'leetcode: Longest Palindromic Substring'
date: 2021-11-03 09:56:46
tags: leetcode,php
---

- 问题

> Given a string s, return the longest palindromic substring in s.



- 代码

```
class Solution {

    /**
     * @param String $s
     * @return String
     */
    function longestPalindrome($s) {
        $len = strlen($s);
        if ($len === 1) {
            return $s;
        }

        if ($len === 2) {
            if ($s[0] === $s[1]) {
                return $s;
            }

            return $s[0];
        }

        $palindromic = $s[0];
        $max = 1;
        for($i = 0; $i < $len; $i++) {
            if (strlen(substr($s, $i)) < $max) {
                break;
            }
            
            for($j = $len - $i - 1; $j >= 0; $j--) {
                if ($j + 1 < $max) {
                    break;
                }

                $str = substr($s, $i, $j + 1);
                $sub = strrev($str);
                if ($sub === $str) {
                    if($max < strlen($sub)) {
                        $palindromic = $sub;
                        $max = strlen($sub);
                    }

                    break;
                }
            }
        }

        return $palindromic;
    }
}
```

- 结果

> 11510/11510 cases passed (90 ms)
> Your runtime beats 9.13 % of php submissions
> Your memory usage beats 99.87 % of php submissions (15.1 MB)