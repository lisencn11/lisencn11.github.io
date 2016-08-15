---
layout: post
title: Leetcode Problem 70 Summary
date: 2016-08-14
categories: blog
tags: [study]

---

# 题目

You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

# 我的算法

DP 问题，第 n 级台阶的方案数等于 n - 1 级的方案数加上 n - 2 级的方案数。

# 代码

{% highlight java %}
public class Solution {
    public int climbStairs(int n) {
        if (n == 0 || n == 1 || n == 2) return n;
        int[] ways = new int[n + 1];
        ways[1] = 1;
        ways[2] = 2;
        for (int i = 3; i <= n; i++)
            ways[i] = ways[i - 1] + ways[i - 2];
        
        return ways[n];
    }
}
{% endhighlight %}