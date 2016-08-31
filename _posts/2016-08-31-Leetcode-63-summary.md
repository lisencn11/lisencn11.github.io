---
layout: post
title: Leetcode Problem 63 Summary
date: 2016-08-31
categories: blog
tags: [study]

---

# 题目

Follow up for "Unique Paths":

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and empty space is marked as 1 and 0 respectively in the grid.

For example,  
There is one obstacle in the middle of a 3x3 grid as illustrated below.

[  
  [0,0,0],  
  [0,1,0],  
  [0,0,0]  
]

# 我的算法

简单动态规划

# 代码

{% highlight java %}
public class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length, n = obstacleGrid[0].length;
        int[][] paths = new int[m][n];
        if (obstacleGrid[0][0] == 1 || obstacleGrid[m - 1][n - 1] == 1) return 0;
        paths[0][0] = 1;
        
        for (int i = 1; i < m; i++)
            paths[i][0] = obstacleGrid[i][0] == 1 ? 0 : paths[i - 1][0];
        for (int j = 1; j < n; j++)
            paths[0][j] = obstacleGrid[0][j] == 1 ? 0 : paths[0][j - 1];
        
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (obstacleGrid[i][j] != 1) paths[i][j] = paths[i - 1][j] + paths[i][j - 1];
                else paths[i][j] = 0;
            }
        }
        
        return paths[m - 1][n - 1];
    }
}
{% endhighlight %}