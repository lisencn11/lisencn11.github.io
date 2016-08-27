---
layout: post
title: Leetcode Problem 48 Summary
date: 2016-08-26
categories: blog
tags: [study]

---

# 题目

You are given an n x n 2D matrix representing an image.

Rotate the image by 90 degrees (clockwise).

Follow up:  
Could you do this in-place?

# 我的算法

矩阵操作。

# 代码

{% highlight java %}
public class Solution {
    public void rotate(int[][] matrix) {
        if (matrix == null || matrix.length == 0) return;
        
        int n = matrix.length;
        int up = 0, down = n - 1;
        
        while (up < down) {
            for (int i = 0; i + up < down; i++) {
                int temp = matrix[down - i][up];
                matrix[down - i][up] = matrix[down][down - i];
                matrix[down][down - i] = matrix[up + i][down];
                matrix[up + i][down] = matrix[up][up + i];
                matrix[up][up + i] = temp;
            }
            
            up++;
            down--;
        }
    }
}
{% endhighlight %}