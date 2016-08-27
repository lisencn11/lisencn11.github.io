---
layout: post
title: Leetcode Problem 64 Summary
date: 2016-08-26
categories: blog
tags: [study]

---

# 题目

Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

Note: You can only move either down or right at any point in time.

# 我的算法

动态规划，当前格子值等于上面和左面的最小值加上当前格子。

# 代码

{% highlight java %}
public class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][] sum = new int[m][n];
        sum[0][0] = grid[0][0];
        for (int i = 1; i < m; i++) sum[i][0] = sum[i - 1][0] + grid[i][0];
        for (int i = 1; i < n; i++) sum[0][i] = sum[0][i - 1] + grid[0][i];
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                sum[i][j] = sum[i - 1][j] > sum[i][j - 1] ? sum[i][j - 1] : sum[i - 1][j];
                sum[i][j] += grid[i][j];
            }
        }
        return sum[m - 1][n - 1];
    }
}
{% endhighlight %}