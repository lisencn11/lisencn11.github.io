---
layout: post
title: Leetcode Problem 54 Summary
date: 2016-08-25
categories: blog
tags: [study]

---

# 题目

Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.

For example,  
Given the following matrix:

[  
 [ 1, 2, 3 ],  
 [ 4, 5, 6 ],  
 [ 7, 8, 9 ]  
]

You should return [1,2,3,6,9,8,7,4,5].

# 我的算法

简单的数组操作。。。

# 代码

{% highlight java %}
public class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        if (matrix == null || matrix.length == 0) return new ArrayList<>();
        List<Integer> list = new ArrayList<>();
        helper(matrix, list, 0, 0, matrix.length - 1, matrix[0].length - 1);
        return list;
    }
    
    private void helper(int[][] matrix, List<Integer> list, int x1, int y1, int x2, int y2) {
        if (x1 > x2 || y1 > y2) return;
        if (x1 == x2) {
            for (int i = y1; i <= y2; i++) {
                list.add(matrix[x1][i]);
            }
            return;
        }
        if (y1 == y2) {
            for (int i = x1; i <= x2; i++) {
                list.add(matrix[i][y1]);
            }
            return;
        }
        
        for (int i = y1; i < y2; i++) list.add(matrix[x1][i]);
        for (int i = x1; i < x2; i++) list.add(matrix[i][y2]);
        for (int i = y2; i > y1; i--) list.add(matrix[x2][i]);
        for (int i = x2; i > x1; i--) list.add(matrix[i][y1]);
        helper(matrix, list, x1 + 1, y1 + 1, x2 - 1, y2 - 1);
    }
}
{% endhighlight %}