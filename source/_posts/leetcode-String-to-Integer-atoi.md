---
title: 'leetcode: String to Integer (atoi)'
date: 2021-10-28 09:02:42
tags: leetcode,php
---

```
class Solution
{

    /**
     * @param String $s
     * @return Integer
     */
    public function myAtoi($s)
    {
        $s = ltrim($s);
        if (empty($s)) {
            return 0;
        }

        $ascii = ord($s[0]);
        $sign  = $ascii === 45;
        if ($ascii === 45 || $ascii === 43) {
            $sum = 0;
        } elseif ($ascii >= 48 && $ascii <= 57) {
            $sum = intval($s[0]);
        } else {
            return 0;
        }

        for ($i = 1, $len = strlen($s); $i < $len; ++$i) {
            $ascii = ord($s[$i]);
            if ($ascii < 48 || $ascii > 57) {
                break;
            }

            $sum = $sum * 10 + intval($s[$i]);
        }

        if ($sign) {
            $sum *= -1;
        }

        $boundry = pow(2, 31);
        if ($sum <= -$boundry) {
            return -$boundry;
        } elseif ($sum >= --$boundry) {
            return $boundry;
        }

        return $sum;
    }
}
```