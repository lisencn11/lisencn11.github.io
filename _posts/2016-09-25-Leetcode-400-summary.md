---
layout: post
title: Leetcode Problem 400 Summary
date: 2016-09-25
categories: blog
tags: [study]

---

# 题目

Find the nth digit of the infinite integer sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...

Note:
n is positive and will fit within the range of a 32-bit signed integer (n < 231).

Example 1:

Input:
3

Output:
3
Example 2:

Input:
11

Output:
0

Explanation:
The 11th digit of the sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ... is a 0, which is part of the number 10.

# 我的算法

一位数有 9 个，二位数有 90 个，以此类推，找到第 n 位是几位数，然后是第几个数，输出即可。

# 代码

{% highlight java %}
public class Solution {
    public int findNthDigit(int n) {
        long count = 9;
        int start = 1;
        int digits = 1;
        
        while (n > count * digits) {
            n -= count * digits;
            digits++;
            start *= 10;
            count *= 10;
        }
        
        start += (n - 1) / digits;
        String s = Integer.toString(start);
        return Character.getNumericValue(s.charAt((n - 1) % digits));
    }
}
{% endhighlight %}