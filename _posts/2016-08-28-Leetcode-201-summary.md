---
layout: post
title: Leetcode Problem 201 Summary
date: 2016-08-28
categories: blog
tags: [study]

---

# 题目

Given a range [m, n] where 0 <= m <= n <= 2147483647, return the bitwise AND of all numbers in this range, inclusive.

For example, given the range [5, 7], you should return 4.

# 我的算法

动态规划，如果 m = n 那么返回 m 就好，m != n 那么问题可以转化为求 m / 2 和 n / 2 的按位与再左移一位。

# 代码

{% highlight java %}
public class Solution {
    public int rangeBitwiseAnd(int m, int n) {
        return m != n ? rangeBitwiseAnd(m / 2, n / 2) << 1 : m;
    }
}
{% endhighlight %}