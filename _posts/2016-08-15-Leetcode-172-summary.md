---
layout: post
title: Leetcode Problem 172 Summary
date: 2016-08-15
categories: blog
tags: [study]

---

# 题目

Given an integer n, return the number of trailing zeroes in n!.

Note: Your solution should be in logarithmic time complexity.

# 我的算法

我观察到了这结果取决于 n! 中能分解出多少个 5 ，但是没有找出好的计算方法，下为 discuss 中的解法。

# 代码

{% highlight java %}
public class Solution {
    public int trailingZeroes(int n) {
        if (n < 5) return 0;
        return trailingZeroes(n / 5) + n / 5;
    }
}
{% endhighlight %}