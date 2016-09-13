---
layout: post
title: Leetcode Problem 85 Summary
date: 2016-09-11
categories: blog
tags: [study]

---

# 题目

Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.

For example, given the following matrix:

1 0 1 0 0  
1 0 1 1 1  
1 1 1 1 1  
1 0 0 1 0  
Return 6.

# 我的算法

DP 动态规划

保存三个数组 left right height

* height 数组中存着当前 matrix[i][j] 向上有多少个连续 1
* left 数组中存着对于当前 matrix[i][j] 的高度向左平移能够到达的边界 (inclusive)
* right 数组中存着对于当前 matrix[i][j] 的高度向右平移能够到达的边界 (exclusive)

所以用 (right[j] - left[j]) * height[j] 可以求得当前高度的最大矩形。

# 代码

{% highlight java %}
public class Solution {
    public int maximalRectangle(char[][] matrix) {
        if (matrix == null || matrix.length == 0) return 0;
        int m = matrix.length, n = matrix[0].length;
        int[] height = new int[n], left = new int[n], right = new int[n];
        Arrays.fill(height, 0);
        Arrays.fill(left, 0);
        Arrays.fill(right, n);
        int max = 0;
        
        for (int i = 0; i < m; i++) {
            int currLeft = 0, currRight = n;
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == '0') {
                    height[j] = 0;
                } else {
                    height[j]++;
                }
            }
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == '0') {
                    currLeft = j + 1;
                    left[j] = 0;
                } else {
                    left[j] = Math.max(left[j], currLeft);
                }
            }
            for (int j = n - 1; j >= 0; j--) {
                if (matrix[i][j] == '0') {
                    currRight = j;
                    right[j] = n;
                } else {
                    right[j] = Math.min(right[j], currRight);
                }
            }
            for (int j = 0; j < n; j++) {
                max = Math.max(max, (right[j] - left[j]) * height[j]);
            }
        }
        return max;
    }
}
{% endhighlight %}