---
layout: post
title: Leetcode Problem 264 Summary
date: 2016-06-08
categories: blog
tags: [study]

---

# 题目

Write a program to find the n-th ugly number.

Ugly numbers are positive numbers whose prime factors only include 2, 3, 5. For example, 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 is the sequence of the first 10 ugly numbers.

Note that 1 is typically treated as an ugly number.

Hint:

1. The naive approach is to call isUgly for every number until you reach the nth one. Most numbers are not ugly. Try to focus your effort on generating only the ugly ones.
2. An ugly number must be multiplied by either 2, 3, or 5 from a smaller ugly number.
3. The key is how to maintain the order of the ugly numbers. Try a similar approach of merging from three sorted lists: L1, L2, and L3.
4. Assume you have Uk, the kth ugly number. Then Uk+1 must be Min(L1 * 2, L2 * 3, L3 * 5).


# 算法

比较粗暴的算法是从2开始挨个测试是否丑数，算法复杂度极低。经过观察发现丑数都有已有丑数乘上2，3，5产生，故可以使用动态规划存储已有k-1个丑数从而产生第k个丑数。难点在于递推关系式：通过分析，发现如果第k-1个丑数为*ugly[k-1]=min(ugly[i1] \* 2, ugly[i2] \* 3, ugly[i3] \* 5)*，且*ugly[k-1]==ugly[i1] \* 2*，则第k个丑数为*ugly[k-1]=min(ugly[i1 + 1] \* 2, ugly[i2] \* 3, ugly[i3] \* 5)*。

# 代码

{% highlight java %}
public class Solution {
    private int min(int i1, int i2, int i3) {
        int min;
        min = Math.min(i1, i2);
        min = Math.min(min, i3);
        return min;
    }
    
    public int nthUglyNumber(int n) {
        int[] res = new int[n];
        int i1 = 0, i2 = 0, i3 = 0;
        res[0] = 1;
        
        for (int i = 1; i < n; i++) {
            res[i] = min(res[i1] * 2, res[i2] * 3, res[i3] * 5);
            if (res[i] == (res[i1] * 2)) i1++;
            if (res[i] == (res[i2] * 3)) i2++;
            if (res[i] == (res[i3] * 5)) i3++;
        }
        
        return res[n - 1];
    }
}
{% endhighlight %}

# 二刷

{% highlight java %}
public class Solution {
    public int nthUglyNumber(int n) {
        int[] ugly = new int[n + 1];
        ugly[1] = 1;
        int[] index = new int[]{1, 1, 1};
        int[] base = new int[]{2, 3, 5};
        int count = 1;
        
        for (int i = 2; i <= n; i++) {
            int min = Integer.MAX_VALUE;
            for (int j = 0; j < 3; j++) {
                min = ugly[index[j]] * base[j] < min ? ugly[index[j]] * base[j] : min;
            }
            ugly[i] = min;
            for (int j = 0; j < 3; j++) {
                if (ugly[index[j]] * base[j] == min) index[j]++;
            }
        }
        return ugly[n];
    }
}
{% endhighlight %}