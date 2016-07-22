---
layout: post
title: Leetcode Problem 29 Summary
date: 2016-07-22
categories: blog
tags: [study]

---

# 题目

在不使用 * / % 三个运算符的情况下进行两个整型的除法

# 我的算法

二分查找思路：从除数 divisor ， divisor * 2 ， divisor * 4 ... 一个一个试直到大于被除数，得到一部分倍率，对剩下的被除数进行相同的算法，这里的乘法可以用移位代替。最后注意一些边界条件即可。

# 代码

{% highlight java %}
public class Solution {
    public int divide(int dividend, int divisor) {
        if (dividend == Integer.MIN_VALUE && divisor == -1) return Integer.MAX_VALUE;
        if (divisor == 0) return Integer.MAX_VALUE;
        if (divisor == 1) return dividend;
        if (divisor == -1) return -dividend;
        
        int cumuTimes = 0;
        long posDividend = (long)Math.abs((long)dividend);
        long posDivisor = (long)Math.abs((long)divisor);

        while (posDivisor <= posDividend) {
            int times = 1;
            long sum = posDivisor;
            while ((sum<<1) < posDividend) {
                sum <<= 1;
                times <<= 1;
            }
            posDividend -= sum;
            cumuTimes += times;
        }
        
        if ((dividend < 0 && divisor > 0) || (dividend > 0 && divisor < 0))
            cumuTimes = -cumuTimes;
        
        return cumuTimes;
    }
}
{% endhighlight %}