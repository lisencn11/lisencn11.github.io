---
layout: post
title: Leetcode Problem 304 Summary
date: 2016-07-15
categories: blog
tags: [study]

---

# 题目

**输入**一个二维整型矩阵。

**输出**自矩阵的和。

注：

1. 矩阵不变
2. 会调用多次求自矩阵的和
3. row1 <= row2，col1 <= col2

如：  
>Given matrix = [  
  [3, 0, 1, 4, 2],  
  [5, 6, 3, 2, 1],  
  [1, 2, 0, 1, 5],  
  [4, 1, 0, 1, 7],  
  [1, 0, 3, 0, 5]  
]

>sumRegion(2, 1, 4, 3) -> 8  
sumRegion(1, 1, 2, 2) -> 11  
sumRegion(1, 2, 2, 4) -> 12

# 我的算法

DP算法，建立一个纪录从当前格子到(0, 0)的自矩阵和的矩阵。对于某一矩阵只需要用右下角的和减去左边和上边，再加上多减一次的左上角即可。

# 代码

{% highlight java %}
public class NumMatrix {
    int[][] sum = null;
    int[][] matrix = null;

    public NumMatrix(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return;
        sum = new int[matrix.length][matrix[0].length];
        this.matrix = matrix;
        
        sum[0][0] = matrix[0][0];
        for (int i = 1; i < matrix[0].length; i++) 
            sum[0][i] = sum[0][i - 1] + matrix[0][i];
        for (int i = 1; i < matrix.length; i++) 
            sum[i][0] = sum[i - 1][0] + matrix[i][0];
        for (int i = 1; i < matrix.length; i++) 
            for (int j = 1; j < matrix[0].length; j++) 
                sum[i][j] = sum[i - 1][j] + sum[i][j - 1] - sum[i - 1][j - 1] + matrix[i][j];
        
    }

    public int sumRegion(int row1, int col1, int row2, int col2) {
        if (sum == null || sum.length == 0 || sum[0].length == 0) return 0;
        if (row1 == 0 && col1 == 0) return sum[row2][col2];
        if (row1 == 0) return sum[row2][col2] - sum[row2][col1 - 1];
        if (col1 == 0) return sum[row2][col2] - sum[row1 - 1][col2];
        return sum[row2][col2] + sum[row1 - 1][col1 - 1] - sum[row2][col1 - 1] - sum[row1 - 1][col2];
    }
}


// Your NumMatrix object will be instantiated and called as such:
// NumMatrix numMatrix = new NumMatrix(matrix);
// numMatrix.sumRegion(0, 1, 2, 3);
// numMatrix.sumRegion(1, 2, 3, 4);
{% endhighlight %}