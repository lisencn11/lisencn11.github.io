---
layout: post
title: Leetcode Problem 405 Summary
date: 2016-09-28
categories: blog
tags: [study]

---

# 题目

Given an integer, write an algorithm to convert it to hexadecimal. For negative integer, two’s complement method is used.

Note:

1. All letters in hexadecimal (a-f) must be in lowercase.
2. The hexadecimal string must not contain extra leading 0s. If the number is zero, it is represented by a single zero character '0'; otherwise, the first character in the hexadecimal string will not be the zero character.
3. The given number is guaranteed to fit within the range of a 32-bit signed integer.
4. You must not use any method provided by the library which converts/formats the number to hex directly.

Example 1:

Input:  
26

Output:  
"1a"

Example 2:

Input:  
-1

Output:  
"ffffffff"

# 我的算法

每次向右平移四位，提取十六进制码

# 代码

{% highlight java %}
public class Solution {
    public String toHex(int num) {
        if (num == 0) return "0";
        String[] map = new String[]{"a", "b", "c", "d", "e", "f"};
        StringBuilder result = new StringBuilder();
        while (num != 0) {
            int digit = 0;
            for (int i = 3; i >= 0; i--) {
                if ((num & (1 << i)) != 0) {
                    digit += (1 << i);
                }
            }
            num = num >>> 4;
            if (digit >= 10) {
                result.insert(0, map[digit - 10]);
            } else {
                result.insert(0, digit);
            }
        }
        return result.toString();
    }
}
{% endhighlight %}