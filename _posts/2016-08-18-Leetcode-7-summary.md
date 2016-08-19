---
layout: post
title: Leetcode Problem 7 Summary
date: 2016-08-18
categories: blog
tags: [study]

---

# 题目

Reverse digits of an integer.

Example1: x = 123, return 321  
Example2: x = -123, return -321

click to show spoilers.

Have you thought about this?  
Here are some good questions to ask before coding. Bonus points for you if you have already thought through this!

If the integer's last digit is 0, what should the output be? ie, cases such as 10, 100.

Did you notice that the reversed integer might overflow? Assume the input is a 32-bit integer, then the reverse of 1000000003 overflows. How should you handle such cases?

For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

Update (2014-11-10):  
Test cases had been added to test the overflow behavior.

# 我的算法

考察严谨性， reverse 后可能溢出？

# 代码

{% highlight java %}
public class Solution {
    public int reverse(int x) {
        long sum = 0;
        
        while (x != 0) {
            int digit = x % 10;
            sum = sum * 10 + (long)digit;
            x /= 10;
        }
        
        if (sum > Integer.MAX_VALUE || sum < Integer.MIN_VALUE) return 0;
        return (int)sum;
    }
}
{% endhighlight %}