---
layout: post
title: Leetcode Problem 396 Summary
date: 2016-09-13
categories: blog
tags: [study]

---

# 题目

Given an array of integers A and let n to be its length.

Assume Bk to be an array obtained by rotating the array A k positions clock-wise, we define a "rotation function" F on A as follow:

F(k) = 0 * Bk[0] + 1 * Bk[1] + ... + (n-1) * Bk[n-1].

Calculate the maximum value of F(0), F(1), ..., F(n-1).

Note:  
n is guaranteed to be less than 105.

Example:

A = [4, 3, 2, 6]

F(0) = (0 * 4) + (1 * 3) + (2 * 2) + (3 * 6) = 0 + 3 + 4 + 18 = 25  
F(1) = (0 * 6) + (1 * 4) + (2 * 3) + (3 * 2) = 0 + 4 + 6 + 6 = 16  
F(2) = (0 * 2) + (1 * 6) + (2 * 4) + (3 * 3) = 0 + 6 + 8 + 9 = 23  
F(3) = (0 * 3) + (1 * 2) + (2 * 6) + (3 * 4) = 0 + 2 + 12 + 12 = 26

So the maximum value of F(0), F(1), F(2), F(3) is F(3) = 26.

# 我的算法

记录 sum ，借助 sum 将 B<sub>k</sub> 进行 O(1) 复杂度的转换。

# 代码

{% highlight java %}
public class Solution {
    public int maxRotateFunction(int[] A) {
        if (A == null || A.length == 0) return 0;
        long base = 0;
        long sum = 0;
        int len = A.length;
        for (int i = 0; i < len; i++) {
            base += (long) A[i] * i;
            sum += (long) A[i];
        }
        
        long max = base;
        for (int i = 0; i < len; i++) {
            base = base + sum - len * A[len - i - 1];
            max = Math.max(max, base);
        }
        return (int) max;
    }
}
{% endhighlight %}