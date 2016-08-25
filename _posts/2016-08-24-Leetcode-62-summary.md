---
layout: post
title: Leetcode Problem 62 Summary
date: 2016-08-24
categories: blog
tags: [study]

---

# 题目

A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

# 我的算法

动态规划：当前位置的解取决于上边格子和左边格子的和。

数学：排列组合问题，向下走 m - 1 步，向右走 n - 1 步，一共有多少种排列组合方法？

![](https://lisencn11.github.io/img/problem62.png)

# 代码

### 动态规划

{% highlight java %}
public class Solution {
    public int uniquePaths(int m, int n) {
        int[][] solution = new int[m][n];
        for (int i = 0; i < m; i++) solution[i][0] = 1;
        for (int i = 0; i < n; i++) solution[0][i] = 1;
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                solution[i][j] = solution[i - 1][j] + solution[i][j - 1];
            }
        }
        return solution[m - 1][n - 1];
    }
}
{% endhighlight %}

### 排列组合

{% highlight java %}
public class Solution {
    public int uniquePaths(int m, int n) {
        if (m == 1 || n == 1) return 1;
        long down = m - 1;
        long right = n - 1;
        long min = down > right ? right : down;
        long total = down + right;
        long denominator = 1;
        long numerator = 1;
        for (int i = 0; i < min; i++) {
            numerator *= total--;
            denominator *= (long) (i + 1);
        }
        
        return (int) (numerator / denominator);
    }
}
{% endhighlight %}