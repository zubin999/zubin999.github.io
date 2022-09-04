---
title: leetcode:3Sum
date: 2021-12-06 15:51:20
tags: leetcode,php
---

- 问题

Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i != j, i != k, and j != k, and nums[i] + nums[j] + nums[k] == 0.

Notice that the solution set must not contain duplicate triplets.



- 代码

根据[投票最高][1]的得到:

```
class Solution
{

    /**
     * @param Integer[] $nums
     * @return Integer[][]
     */
    public function threeSum($nums)
    {
        $len = count($nums);
        if ($len < 3) {
            return [];
        }

        sort($nums, SORT_ASC);
        if ($nums[0] > 0) {
            return [];
        }

        $res = [];
        for ($i = 0, $len1 = $len - 2; $i < $len1; $i++) {
            if ($i === 0 || ($i > 0 && $nums[$i] !== $nums[$i - 1])) {
                $lo  = $i + 1;
                $hi  = $len - 1;
                $sum = 0 - $nums[$i];

                while ($lo < $hi) {
                    $third = $nums[$lo] + $nums[$hi];
                    if ($third === $sum) {
                        $res[] = [$nums[$i], $nums[$lo], $nums[$hi]];

                        while ($lo < $hi && $nums[$lo] === $nums[$lo + 1]) {
                            $lo++;
                        }

                        while ($lo < $hi && $nums[$hi] === $nums[$hi - 1]) {
                            $hi--;
                        }

                        $lo++;
                        $hi--;
                        continue;
                    }

                    if ($third < $sum) {
                        $lo++;
                        continue;
                    }

                    $hi--;
                }
            }
        }

        return $res;
    }
}
```

- 结果

> 318/318 cases passed (180 ms)
> Your runtime beats 95.24 % of php submissions
> Your memory usage beats 85.71 % of php submissions (24.9 MB)

  [1]: https://leetcode.com/problems/3sum/discuss/7380/Concise-O(N2)-Java-solution