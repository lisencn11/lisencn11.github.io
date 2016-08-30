---
layout: post
title: Leetcode Problem 329 Summary
date: 2016-08-28
categories: blog
tags: [study]

---

# 题目

Given an integer matrix, find the length of the longest increasing path.

From each cell, you can either move to four directions: left, right, up or down. You may NOT move diagonally or move outside of the boundary (i.e. wrap-around is not allowed).

Example 1:

nums = [  
  [9,9,4],  
  [6,6,8],  
  [2,1,1]  
]  
Return 4  
The longest increasing path is [1, 2, 6, 9].

Example 2:

nums = [  
  [3,4,5],  
  [3,2,6],  
  [2,2,1]  
]  
Return 4  
The longest increasing path is [3, 4, 5, 6]. Moving diagonally is not allowed.

# 我的算法

DFS + DP

# 代码

{% highlight java %}
public class Solution {
    public int longestIncreasingPath(int[][] matrix) {
        if (matrix == null || matrix.length == 0) return 0;
        int m = matrix.length, n = matrix[0].length;
        int[][] map = new int[m][n];
        int max = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int len = dfs(map, matrix, i, j);
                max = len > max ? len : max;
            }
        }
        return max;
    }
    
    private int dfs(int[][] map, int[][] matrix, int row, int col) {
        if (map[row][col] != 0) return map[row][col];
        int count = 0;
        int max = 1;
        if (row > 0 && matrix[row][col] < matrix[row - 1][col]) {
            int temp = dfs(map, matrix, row - 1, col) + 1;
            max = temp > max ? temp : max;
            count++;
        }
        if (row < matrix.length - 1 && matrix[row][col] < matrix[row + 1][col]) {
            int temp = dfs(map, matrix, row + 1, col) + 1;
            max = temp > max ? temp : max;
            count++;
        }
        if (col > 0 && matrix[row][col] < matrix[row][col - 1]) {
            int temp = dfs(map, matrix, row , col - 1) + 1;
            max = temp > max ? temp : max;
            count++;
        }
        if (col < matrix[0].length - 1 && matrix[row][col] < matrix[row][col + 1]) {
            int temp = dfs(map, matrix, row, col + 1) + 1;
            max = temp > max ? temp : max;
            count++;
        }
        map[row][col] = max;
        return max;
    }
}
{% endhighlight %}