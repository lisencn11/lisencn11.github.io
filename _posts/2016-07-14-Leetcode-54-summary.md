---
layout: post
title: Leetcode Problem 54 Summary
date: 2016-07-14
categories: blog
tags: [study]

---

# 题目

**输入**一个 m * n 整型数组。

**输出**螺旋顺序输出数组元素。

如：  
>Given the following matrix:  
[  
 [ 1, 2, 3 ],  
 [ 4, 5, 6 ],  
 [ 7, 8, 9 ]  
]  
You should return [1,2,3,6,9,8,7,4,5].

# 我的算法

使用for循环一圈一圈遍历，限定当前圈的四个角即可，最后注意处理特例，当前圈的行数或列数为0。

# 代码

{% highlight java %}
public class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return new ArrayList<>();
        
        List<Integer> ret = new ArrayList<>();
        int cycle = (Math.min(matrix.length, matrix[0].length) + 1) / 2;
        int row = matrix.length;
        int col = matrix[0].length;
        
        for (int i = 0; i < cycle; i++) {
            if (col - i - 1 == i) {
                for (int j = i; j <= row - i - 1; j++) {
                    ret.add(matrix[j][i]);
                }
            } else if (row - i - 1 == i) {
                for (int j = i; j <= col - i - 1; j++) {
                    ret.add(matrix[i][j]);
                }
            } else {
                for (int j = i; j < col - i - 1; j++)
                    ret.add(matrix[i][j]);
                for (int j = i; j < row - i - 1; j++)
                    ret.add(matrix[j][col - i - 1]);
                for (int j = col - i - 1; j > i; j--)
                    ret.add(matrix[row - i - 1][j]);
                for (int j = row - i - 1; j > i; j--)
                    ret.add(matrix[j][i]);
            }
        }
        
        return ret;
    }
}
{% endhighlight %}