---
title: leetcode:Valid Parentheses
date: 2022-04-11 21:34:38
tags:
---

- 问题

> Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.
>
> An input string is valid if:
>
> 1. Open brackets must be closed by the same type of brackets.
> 2. Open brackets must be closed in the correct order.


- 解决

```
class Solution {

    /**
     * @param String $s
     * @return Boolean
     */
    function isValid($s) {
        if(strlen($s) % 2) {
            return false;
        }

        while(true){
            $c = $s[0];
            $s = substr($s, 1);
            $i = $this->match($c, $s);
            if($i === false){
                return false;
            }

            $s = substr($s, 0, $i) . substr($s, $i + 1);
            if(empty($s)){
                break;
            }
        }

        return true;
    }

    function match($c, $s)
    {
        $map = [
            '(' => ')',
            '{' => '}',
            '[' => ']'
        ];

        if($s[0] === $map[$c]) {
            return 0;
        }

        $offset = 0;
        for($i = 0, $len = strlen($s); $i < $len; $i++) {
            if(in_array($s[$i], ['(', '{', '['], true)) {
                $offset += 2;
            }

            if($s[$i] === $map[$c] && $offset === $i) {
                return $offset;
            }
        }

        return false;
    }
}
```

主要找到对应位置,然后比较对应位置是否正确并不断将正确的从字符串中移除.运行效率很低,500ms~900ms之间.
看社区排名前面的解决方法(python,java,c++)都是通过栈来解决--先进后出.修改如下:

```
class Solution {

    /**
     * @param String $s
     * @return Boolean
     */
    function isValid($s)
    {
        $stack = [];

        $map = ['(' => ')', '{' => '}', '[' => ']'];
        for($i = 0, $len = strlen($s); $i < $len; $i++) {

            if(isset($map[$s[$i]])) {
                $stack[] = $map[$s[$i]];
                continue;
            }
            
            if(in_array($s[$i], $map, true)) {
                if($stack === []) {
                    return false;
                }

                $last = array_pop($stack);
                if($s[$i] !== $last) {
                    return false;
                }
            }
        }

        return $stack === [];
    }
}
```

运行时间最快只需8ms以内.