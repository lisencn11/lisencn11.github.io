---
layout: post
title: Leetcode Problem 213 Summary
date: 2016-08-22
categories: blog
tags: [study]

---

# 题目

Note: This is an extension of House Robber.

After robbing those houses on that street, the thief has found himself a new place for his thievery so that he will not get too much attention. This time, all houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, the security system for these houses remain the same as for those in the previous street.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

# 我的算法

198 题中我们知道如何解最简单的房子排成一排的情况，那么这道题唯一的问题就是改成 circle 后最后一个节点和第一个节点可能被同时取到，那么我们分成两种情况：

1. 我们可以取第一个结点，那么这种情况下最后一个节点不能取。
2. 我们不取第一个结点，那么最后一个结点可以取。

比较上述两种情况的大小即可。

# 代码

{% highlight java %}
public class Solution {
    public String intToRoman(int num) {
        String[] roman = {"I", "IV", "V", "IX", "X", "XL", "L", "XC", "C", "CD", "D", "CM", "M"};
        int[] number = {1, 4, 5, 9, 10, 40, 50, 90, 100, 400, 500, 900, 1000};
        StringBuilder ret = new StringBuilder();
        int remain = num;
        
        for (int i = number.length - 1; i >= 0; i--) {
            while (remain >= number[i]) {
                remain -= number[i];
                ret.append(roman[i]);
            }
            if (remain == 0) break;
        }
        
        return ret.toString();
    }
}
{% endhighlight %}