---
title: 'leetcode: ZigZag Conversion'
date: 2021-10-15 15:22:22
tags: leetcode,php,ZigZag
---

```
/*
 * @lc app=leetcode id=6 lang=php
 *
 * [6] ZigZag Conversion
 */

// @lc code=start
class Solution {

    /**
     * @param String $s
     * @param Integer $numRows
     * @return String
     */
    function convert($s, $numRows) {
        $len = strlen($s);
        if ($len === 1 || $numRows === 1) {
            return $s;
        }

        $max = 2 * $numRows - 2;
        
        $str = '';
        // 0 4 8 12
        // 1 3 5 7 9 11 13
        // 2 6 10

        // 0   6    12
        // 1 5 7 11 13
        // 2 4 8 10
        // 3   9    [15]

        // P   H
        // A S I
        // Y I R
        // P L I G
        // A   N
        // 0   8
        // 1 7 9
        // 2 6 10
        // 3 5 11 13
        // 4   12
        
        //ABCDEFGHIJKLMNOPQRSTUVWXYZ
        //A   E   I   M   Q   U   Y
        //B D F H J L N P R T V X Z
        //C   G   K   O   S   W
        //0   4   8    12    16 --- 4
        //1 3 5 7 9 11 13 15 17 --- 2
        //2   6   10   14    18 --- 4
        
        //A   G   M
        //B F H L N R
        //C E I K O Q
        //D   J   P
        //0   6    12    18    6
        //1 5 7 11 13 17 19    4
        //2 4 8 10 14 16 20    2
        //3   9    15    21

        //A   I   Q
        //B H J P R
        //C G K O S
        //D F L N T
        //E   M   U
        //0   8     16    24    8
        //1 7 9  15 17 23 25    6 2 6 2
        //2 6 10 14 18 22       4 4 4 4
        //3 5 11 13 19 21       2 6 2 6
        //4   12    20          8

        //A   K   U
        //B J L T V
        //C I M S W
        //D H N R
        //E G O Q
        //F   P
        //0   10    20   10
        //1 9 11 19 21   8 2 8 2
        //2 8 12 18 22   6 4 6 4
        //3 7 13 17 23   4 6 4 6
        //4 6 14 16 24   2 8 2 8
        //5   15    25   10
        $arr[] = [0, $max];
        $j = 2;
        while($max - $j > 0) {
            $arr[] = [$max - $j, $j];
            $j += 2;
        }
        $arr[] = [0, $max];

        for ($i = 0; $i < $numRows; $i++) {
            $str .= $s[$i];
            $idx = $i;
            while(true) {
                foreach($arr[$i] as $val) {
                    $idx += $val;
                    if($idx >= $len) {
                        break 2;
                    }
                    
                    if (!$val) {
                        continue;
                    }

                    $str .= $s[$idx];
                }
            }
        }

        return $str;
    }
}
// @lc code=end


```