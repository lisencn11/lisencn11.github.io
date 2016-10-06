---
layout: post
title: Leetcode Problem 74 Summary
date: 2016-08-27
categories: blog
tags: [study]

---

# 题目

Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

* Integers in each row are sorted from left to right.
* The first integer of each row is greater than the last integer of the previous row.
* 
For example,

Consider the following matrix:

[  
  [1,   3,  5,  7],  
  [10, 11, 16, 20],  
  [23, 30, 34, 50]  
]

# 我的算法

先对行标做 binary search ， 再对列标做 binary search。

# 代码

{% highlight java %}
public class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length, n = matrix[0].length;
        if (target < matrix[0][0] || target > matrix[m - 1][n - 1]) return false;
        
        int rowL = 0;
        int rowH = m - 1;
        int colL = 0;
        int colH = n - 1;
        
        while (rowL < rowH) {
            int mid = rowL + (rowH - rowL) / 2;
            if (matrix[mid][0] == target) return true;
            else if (matrix[mid][0] > target) rowH = mid - 1;
            else rowL = mid + 1;
        }
        
        rowL = matrix[rowL][0] > target ? rowL - 1 : rowL;
        
        while (colL < colH) {
            int mid = colL + (colH - colL) / 2;
            if (matrix[rowL][mid] == target) return true;
            else if (matrix[rowL][mid] > target) colH = mid - 1;
            else colL = mid + 1;
        }
        return matrix[rowL][colL] == target;
    }
}
{% endhighlight %}

# 二刷

{% highlight java %}
public class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length, n = matrix[0].length;
        int low = 0, high = m - 1;
        int row = 0;
        
        if (target < matrix[0][0] || target > matrix[m - 1][n - 1]) {
            return false;
        }
        
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (matrix[mid][0] == target) {
                return true;
            } else if (matrix[mid][0] < target) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        
        row = low - 1;
        low = 0;
        high = n - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (matrix[row][mid] == target) {
                return true;
            } else if (matrix[row][mid] < target) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        return false;
    }
}
{% endhighlight %}