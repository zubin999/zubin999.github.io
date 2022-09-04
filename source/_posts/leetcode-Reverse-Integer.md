---
title: 'leetcode: Reverse Integer'
date: 2021-10-20 10:08:45
tags: leetcode,php,reverse
---

```
class Solution
{

    /**
     * @param Integer $x
     * @return Integer
     */
    public function reverse($x)
    {
        if ($x < 10 && $x > -10) {
            return $x;
        }

        $reverse = 0;
        $unsign  = $x >= 0;
        $x       = abs($x);
        do {
            $last    = $x % 10;
            $reverse = $reverse * 10 + $last;
            $x       = intval($x / 10);
        } while ($x >= 10);
        $reverse = $reverse * 10 + $x;
        if (!$unsign) {
            $reverse = -$reverse;

            return $reverse < -(pow(2, 31)) ? 0 : $reverse;
        }

        return $reverse > (pow(2, 31) - 1) ? 0 : $reverse;
    }
}
```