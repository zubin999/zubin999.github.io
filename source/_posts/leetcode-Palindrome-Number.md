---
title: 'leetcode: Palindrome Number'
date: 2021-10-28 09:00:17
tags: leetcode,php,paligdrome
---



```
class Solution {

    /**
     * @param Integer $x
     * @return Boolean
     */
    function isPalindrome($x) {
        if($x >= 0 && $x < 10){
            return true;
        }

        if ($x < 0 || $x % 10 === 0) {
            return false;
        }


        return $this->reverse($x) === $x;
    }

    function reverse($x){
        $sum = 0;
        while($x >= 10){
            $last = $x % 10;
            $x = intval($x / 10);
            $sum = $sum * 10 + $last;
        }

        return $sum * 10 + $x;
    }
}
```
