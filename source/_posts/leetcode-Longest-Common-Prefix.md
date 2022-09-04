---
title: leetcode:Longest Common Prefix
date: 2021-11-26 17:45:15
tags: leetcode,php
---

- 问题

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".


- 代码

```
class Solution
{

    /**
     * @param String[] $strs
     * @return String
     */
    public function longestCommonPrefix($strs)
    {
        $prefix = '';

        $min = min(array_map('strlen', $strs));
        if (!$min) {
            return $prefix;
        }

        for ($i = 0; $i < $min; $i++) {
            $alpha = $strs[0][$i];
            for ($j = 1, $b = count($strs); $j < $b; $j++) {
                if ($strs[$j][$i] !== $alpha) {
                    return $prefix;
                }
            }

            $prefix .= $alpha;
        }

        return $prefix;
    }
}
```

- 结果

> 123/123 cases passed (5 ms)
> Your runtime beats 85.19 % of php submissions
> Your memory usage beats 60.49 % of php submissions (15.7 MB)