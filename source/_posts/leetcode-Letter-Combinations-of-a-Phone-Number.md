---
title: leetcode:Letter Combinations of a Phone Number
date: 2022-03-16 19:38:02
tags: leetcode,php
---

- 问题

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.



- 结果

```
class Solution {

    public $keyboard = [
            '2' => ['a', 'b', 'c'],
            '3' => ['d', 'e', 'f'],
            '4' => ['g', 'h', 'i'],
            '5' => ['j', 'k', 'l'],
            '6' => ['m', 'n', 'o'],
            '7' => ['p', 'q', 'r', 's'],
            '8' => ['t', 'u', 'v'],
            '9' => ['w', 'x', 'y', 'z'],
    ];

    /**
     * @param String $digits
     * @return String[]
     */
    function letterCombinations($digits) {
        if(!$digits) {
            return [];
        }

        $len = strlen($digits);
        if($len === 1) {
            return $this->keyboard[$digits];
        }
        
        $first = $this->keyboard[$digits[0]];
        $i = 1;

        do{
            $str = substr($digits, $i++, 1);
            if(empty($str)){
                break;
            }

            $first = $this->combination($first, $str);
        }while(true);

        return $first;
    }

    function combination($arr1, $str)
    {
        $res = [];
        $arr = $this->keyboard[$str];
        foreach ($arr1 as $ch) {
            foreach($arr as $item) {
                $res[] = $ch . $item;
            }
        }

        return $res;
    }
}
```

这个题根据数字要列出所有的字母组合,假如输入`239`,那么需要用`2`对应的每一个字母先和`3`所有的字母组合,然后再和`9`所有的字母组合,如果再有数字,那么接着和下一个数字的所有字母组合.可以发现这里一直在重复着用上一个字母组合数组去和下一个字母数组组合成一个新的数组直到结束.