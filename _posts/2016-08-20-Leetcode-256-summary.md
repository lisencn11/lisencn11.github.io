---
layout: post
title: Leetcode Problem 256 Summary
date: 2016-08-20
categories: blog
tags: [study]

---

# 题目

There are a row of n houses, each house can be painted with one of the three colors: red, blue or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a n x 3 cost matrix. For example, costs[0][0] is the cost of painting house 0 with color red; costs[1][2] is the cost of painting house 1 with color green, and so on... Find the minimum cost to paint all houses.

Note:  
All costs are positive integers.

# 我的算法

动态规划问题，用 minCosts[i][j] 表示第 i 栋房子用第 j 个颜色的情况下，从 0 到 i 的最小值。

# 代码

{% highlight java %}
public class Solution {
    public int minCost(int[][] costs) {
        if (costs == null || costs.length == 0) return 0;
        int len = costs.length;
        int[][] minCosts = new int[len][costs[0].length];
        for (int i = 0; i < costs[0].length; i++) minCosts[0][i] = costs[0][i];
        
        for (int i = 1; i < len; i++) {
            minCosts[i][0] = Math.min(minCosts[i - 1][1], minCosts[i - 1][2]) + costs[i][0];
            minCosts[i][1] = Math.min(minCosts[i - 1][0], minCosts[i - 1][2]) + costs[i][1];
            minCosts[i][2] = Math.min(minCosts[i - 1][0], minCosts[i - 1][1]) + costs[i][2];
        }
        
        return Math.min(Math.min(minCosts[len - 1][0], minCosts[len - 1][1]), minCosts[len - 1][2]);
    }
}
{% endhighlight %}