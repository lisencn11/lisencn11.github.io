---
layout: post
title: Leetcode Problem 231 Summary
date: 2016-08-14
categories: blog
tags: [study]

---

# 题目

Given an integer, write a function to determine if it is a power of two.

# 我的算法

326题为求 Power of Three，两题解法相同。此题又多一个解法，即 n & (n - 1) 即可，因为只有一位为 1 。

# 代码

{% highlight java %}
public class Solution {
    public boolean isPowerOfTwo(int n) {
        if (n < 1) return false;
        return (Math.log10((double)n) / Math.log10((double)2)) % 1 == 0;
    }
}
{% endhighlight %}