---
layout: post
title: Leetcode Problem 279 Summary
date: 2016-08-28
categories: blog
tags: [study]

---

# 题目

Given a positive integer n, find the least number of perfect square numbers (for example, 1, 4, 9, 16, ...) which sum to n.

For example, given n = 12, return 3 because 12 = 4 + 4 + 4; given n = 13, return 2 because 13 = 4 + 9.

# 我的算法

动态规划

# 代码

{% highlight java %}
public class Solution {
    public int numSquares(int n) {
        int[] number = new int[n + 1];
        List<Integer> list = new ArrayList<>();
        Arrays.fill(number, Integer.MAX_VALUE);
        for (int i = 1; i * i <= n; i++) {
            number[i * i] = 1;
            list.add(i * i);
        }
        
        for (int i = 1; i <= n; i++) {
            if (number[i] != Integer.MAX_VALUE) continue;
            for (int j = 0; j < list.size() && i - list.get(j) > 0; j++) {
                number[i] = number[i] > number[i - list.get(j)] + 1 ? number[i - list.get(j)] + 1 : number[i];
            }
        }
        
        return number[n];
    }
}
{% endhighlight %}