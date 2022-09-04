---
title: leetcode:Integer to Roman
date: 2021-11-25 17:34:59
tags: leetcode,php
---



- 题目

Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

```
I    1
V    5
X    10
L    50
C    100
D    500
M    1000
```


- 代码

```
class Solution
{

    /**
     * @param Integer $num
     * @return String
     */
    public function intToRoman($num)
    {
        $roman = '';

        $nine   = ['', 'CM', 'XC', 'IX'];
        $four   = ['', 'CD', 'XL', 'IV'];
        $greate = ['', 'D', 'L', 'V'];
        $less   = ['M', 'C', 'X', 'I'];
        $steps  = [1000, 100, 10, 1];

        $i = 0;
        while ($i < 4) {
            $x = intval($num / $steps[$i]);
            if ($x < 1) {
                $i++;
                continue;
            }
            $num = $num % $steps[$i];

            $times = $x % 5;
            if ($times === 4) {
                if ($x > 5) {
                    $roman .= $nine[$i];
                    $i++;
                    continue;
                }

                $roman .= $four[$i];
                $i++;
                continue;
            }

            if ($x >= 5) {
                $roman .= $greate[$i] . str_repeat($less[$i], $times);
                $i++;
                continue;
            }

            $roman .= str_repeat($less[$i], $times);
            $i++;
        }

        return $roman;
    }
}
```

- 结果

> Accepted
> 3999/3999 cases passed (20 ms)
> Your runtime beats 70.13 % of php submissions
> Your memory usage beats 94.81 % of php submissions (15.4 MB)