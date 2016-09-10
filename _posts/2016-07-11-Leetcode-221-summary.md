---
layout: post
title: Leetcode Problem 221 Summary
date: 2016-07-11
categories: blog
tags: [study]

---

# 题目

Given a 2D binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.

For example, given the following matrix:

1 0 1 0 0  
1 0 1 1 1  
1 1 1 1 1  
1 0 0 1 0

Return 4.

# 我的算法

动态规划算法，用另一个矩阵纪录以当前格子为右下角的正方形的最大面积，若若左上角，上面，左面和当前格子都不为0，说明当前格子面积至少为4，否则为原矩阵值。当前格子的值通过取左上，左，上三个格子中的最小值，开平方后加1在平方得到。需要注意的是输入的是一个char矩阵，所以需要转换成int矩阵。

# 代码

{% highlight java %}
public class Solution {
    public int maximalSquare(char[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return 0;
        }
        
        int[][] area = new int[matrix.length][matrix[0].length];
        int ret = -1;
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[0].length; j++) {
                area[i][j] = (int)(matrix[i][j] - '0');
                ret = area[i][j] > ret ? area[i][j] : ret;
            }
        }
            
        for (int i = 1; i < area.length; i++) {
            for (int j = 1; j < area[0].length; j++) {
                if (area[i - 1][j - 1] != 0 
                     && area[i - 1][j] != 0 
                     && area[i][j - 1] != 0
                     && area[i][j] != 0) {
                    int base = Math.min(area[i - 1][j - 1], 
                        Math.min(area[i - 1][j], area[i][j - 1]));
                    area[i][j] = base + 1;
                    ret = area[i][j] > ret ? area[i][j] : ret;
                }
            }
        }
        
        return ret * ret;
    }
}
{% endhighlight %}

# 二刷

{% highlight java %}
public class Solution {
    public int maximalSquare(char[][] matrix) {
        if (matrix == null || matrix.length == 0) return 0;
        int m = matrix.length, n = matrix[0].length;
        int[][] area = new int[m][n];
        int max = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                area[i][j] = (int) (matrix[i][j] - '0');
                max = area[i][j] > max ? area[i][j] : max;
            }
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                int leftUpper = area[i - 1][j - 1];
                int left = area[i][j - 1];
                int upper = area[i - 1][j];
                area[i][j] = area[i][j] == 0 ? 0 : Math.min(Math.min(leftUpper, left), upper) + 1;
                max = area[i][j] > max ? area[i][j] : max;
            }
        }
        return max * max;
    }
}
{% endhighlight %}