---
title: leetcode:4Sum
date: 2022-03-29 21:36:20
tags: leetcode,php,4sum
---

- 问题

>  Given an array nums of n integers, return an array of all the unique quadruplets [nums[a], nums[b], nums[c], nums[d]] such that:

0 <= a, b, c, d < n
a, b, c, and d are distinct.
nums[a] + nums[b] + nums[c] + nums[d] == target
You may return the answer in any order.

- 难度

中等

- 代码

```
class Solution
{

    /**
     * @param Integer[] $nums
     * @param Integer $target
     * @return Integer[][]
     */
    public function fourSum($nums, $target)
    {
        $len = count($nums);
        if (!$len) {
            return [];
        }

        sort($nums);

        $res = [];
        for ($i = 0, $l1 = $len - 3; $i < $l1; $i++) {
            if ($i && $nums[$i] === $nums[$i - 1]) {
                continue;
            }

            $sum = $target - $nums[$i];
            for ($j = $i + 1, $len1 = $len - 2; $j < $len1; $j++) {
                if ($j === ($i + 1) || $nums[$j] !== $nums[$j - 1]) {
                    $x = $j + 1;
                    $y = $len - 1;

                    while ($x < $y) {
                        $s = $nums[$j] + $nums[$x] + $nums[$y];
                        if ($s === $sum) {
                            $res[] = [$nums[$i], $nums[$j], $nums[$x], $nums[$y]];
                            $x++;
                        } else if ($s > $sum) {
                            $y--;
                        } else {
                            $x++;
                        }

                        while ($x < $y && ($x - 1) > $j && $nums[$x] === $nums[$x - 1]) {
                            $x++;
                        }

                        while ($x < $y && $nums[$y] === $nums[$y + 1]) {
                            $y--;
                        }

                    }
                }
            }
        }

        return $res;
    }
}
```

这题思路和前面的求3个数差不多,也是利用那里的方法来解,开始在`[-2, -1, 0, 0, 1, 2]`这个测试没有通过,少了一个答案`[-1,0,0,1]`, 经过一番测试, 发现在最后判断`$x`相同值时需要加上`($x - 1) > $j`条件.
