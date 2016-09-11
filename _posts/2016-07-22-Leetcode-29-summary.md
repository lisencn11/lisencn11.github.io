---
layout: post
title: Leetcode Problem 29 Summary
date: 2016-07-22
categories: blog
tags: [study]

---

# 题目

Divide two integers without using multiplication, division and mod operator.

If it is overflow, return MAX_INT.

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

# 二刷

{% highlight java %}
public class Solution {
    public int divide(int dividend, int divisor) {
        boolean oppo = false;
        if ((dividend > 0 && divisor < 0) || (dividend < 0 && divisor > 0)) oppo = true;
        
        long up = Math.abs((long) dividend);
        long down = Math.abs((long) divisor);
        long result = 0;
        long sum = 0;
        while (sum + down <= up) {
            long currResult = 1;
            long currSum = down;
            long preResult = 0;
            long preSum = 0;
            while (sum + currSum <= up) {
                preResult = currResult;
                preSum = currSum;
                currSum += currSum;
                currResult += currResult;
            }
            sum += preSum;
            result += preResult;
        }
        
        if (oppo) result = -result;
        if (result > Integer.MAX_VALUE || result < Integer.MIN_VALUE) return Integer.MAX_VALUE;
        else return (int) result;
    }
}
{% endhighlight %}