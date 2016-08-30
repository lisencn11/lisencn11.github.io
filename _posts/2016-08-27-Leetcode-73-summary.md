---
layout: post
title: Leetcode Problem 73 Summary
date: 2016-08-27
categories: blog
tags: [study]

---

# 题目

Given a m x n matrix, if an element is 0, set its entire row and column to 0. Do it in place.

click to show follow up.

Follow up:

Did you use extra space?  
A straight forward solution using O(mn) space is probably a bad idea.  
A simple improvement uses O(m + n) space, but still not the best solution.  
Could you devise a constant space solution?

# 我的算法

用 firstRow 变量标记第一行是否有 0， firstCol 变量标记第一列是否有 0。然后用每一列的第一行的元素标记当前列是否全部置为 0 ，每一行的第一列的元素标记当前行是否全部置为 0.

# 代码

{% highlight java %}
public class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        int firstRow = 1, firstCol = 1;
        for (int i = 0; i < m; i++) {
            if (matrix[i][0] == 0) {
                firstCol = 0;
                break;
            }
        }
        for (int i = 0; i < n; i++) {
            if (matrix[0][i] == 0) {
                firstRow = 0;
                break;
            }
        }
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    matrix[0][j] = 0;
                    matrix[i][0] = 0;
                }
            }
        }
        
        for (int i = 1; i < m; i++)
            if (matrix[i][0] == 0)
                for (int j = 1; j < n; j++) matrix[i][j] = 0;

        for (int j = 1; j < n; j++)
            if (matrix[0][j] == 0)
                for (int i = 1; i < m; i++) matrix[i][j] = 0;
        
        if (firstRow == 0)
            for (int i = 0; i < n; i++) matrix[0][i] = 0;
        
        if (firstCol == 0)
            for (int i = 0; i < m; i++) matrix[i][0] = 0;
    }
}
{% endhighlight %}

# discuss

思路相同，但是难以置信的短。。。

{% highlight java %}
void setZeroes(vector<vector<int> > &matrix) {
    int col0 = 1, rows = matrix.size(), cols = matrix[0].size();

    for (int i = 0; i < rows; i++) {
        if (matrix[i][0] == 0) col0 = 0;
        for (int j = 1; j < cols; j++)
            if (matrix[i][j] == 0)
                matrix[i][0] = matrix[0][j] = 0;
    }

    for (int i = rows - 1; i >= 0; i--) {
        for (int j = cols - 1; j >= 1; j--)
            if (matrix[i][0] == 0 || matrix[0][j] == 0)
                matrix[i][j] = 0;
        if (col0 == 0) matrix[i][0] = 0;
    }
}
{% endhighlight %}