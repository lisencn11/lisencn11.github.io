---
layout: post
title: Leetcode Problem 59 Summary
date: 2016-08-25
categories: blog
tags: [study]

---

# 题目

Given an integer n, generate a square matrix filled with elements from 1 to n2 in spiral order.

For example,  
Given n = 3,

You should return the following matrix:  
[  
 [ 1, 2, 3 ],  
 [ 8, 9, 4 ],  
 [ 7, 6, 5 ]  
]

# 我的算法

简单的数组操作。。。

# 代码

{% highlight java %}
public class Solution {
    public int[][] generateMatrix(int n) {
        if (n == 0) return new int[0][0];
        int count = 1;
        int[][] matrix = new int[n][n];
        int x1 = 0, y1 = 0, x2 = n - 1, y2 = n - 1;
        
        while (x1 <= x2 && y1 <= y2) {
            if (x1 == x2) {
                for (int i = y1; i <= y2; i++) {
                    matrix[x1][i] = count++;
                }
            } else if (y1 == y2) {
                for (int i = x1; i <= x2; i++) {
                    matrix[i][y1] = count++;
                }
            } else {
                for (int i = y1; i < y2; i++) matrix[x1][i] = count++;
                for (int i = x1; i < x2; i++) matrix[i][y2] = count++;
                for (int i = y2; i > y1; i--) matrix[x2][i] = count++;
                for (int i = x2; i > x1; i--) matrix[i][y1] = count++;
            }
            x1++;
            x2--;
            y1++;
            y2--;
        }
        
        return matrix;
    }
}
{% endhighlight %}