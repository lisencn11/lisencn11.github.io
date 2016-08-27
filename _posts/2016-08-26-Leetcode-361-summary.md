---
layout: post
title: Leetcode Problem 361 Summary
date: 2016-08-26
categories: blog
tags: [study]

---

# 题目

Given a 2D grid, each cell is either a wall 'W', an enemy 'E' or empty '0' (the number zero), return the maximum enemies you can kill using one bomb.

The bomb kills all the enemies in the same row and column from the planted point until it hits the wall since the wall is too strong to be destroyed.

Note that you can only put the bomb at an empty cell.

Example:  
For the given grid

0 E 0 0  
E 0 W E  
0 E 0 0

return 3. (Placing a bomb at (1,1) kills 3 enemies)

# 我的算法

如果遍历二维数组并对每一个 0 单独，复杂度是 O((m + n)mn)，而这道题最佳复杂度是 O(mn) ，即我们至少要遍历一遍全部二维数组。

考虑算法，对一行的进行统一更新，即对两个 'W' 之间的 0 更新相同的增量，比如 W0E0E0W 中的三个 0 都需要加 2 。

# 代码

{% highlight java %}
public class Solution {
    public int maxKilledEnemies(char[][] grid) {
        if (grid == null || grid.length == 0) return 0;
        int m = grid.length, n = grid[0].length;
        int[][] matrix = new int[m][n];
        
        for (int i = 0; i < m; i++) {
            int col = 0;
            int pre = 0;
            while (col < n) {
                int count = 0;

                while (col < n && grid[i][col] != 'W') {
                    count = grid[i][col] == 'E' ? count + 1 : count;
                    col++;
                }
                for (int j = pre; j < col; j++) {
                    if (grid[i][j] != 'E' && grid[i][j] != 'W') matrix[i][j] += count;
                }
                pre = col + 1;
                col++;
            }
        }
        
        int max = 0;
        for (int i = 0; i < n; i++) {
            int row = 0;
            int pre = 0;
            while (row < m) {
                int count = 0;
                
                while (row < m && grid[row][i] != 'W') {
                    count = grid[row][i] == 'E' ? count + 1 : count;
                    row++;
                }
                for (int j = pre; j < row; j++) {
                    if (grid[j][i] != 'E' && grid[j][i] != 'W') {
                        matrix[j][i] += count;
                        max = matrix[j][i] > max ? matrix[j][i] : max;
                    }
                }
                pre = row + 1;
                row++;
            }
        }
        return max;
    }
}
{% endhighlight %}

# discuss

同样的复杂度，更简洁的代码和更小的空间复杂度。

{% highlight cpp %}
int maxKilledEnemies(vector<vector<char>>& grid) {
    int m = grid.size(), n = m ? grid[0].size() : 0;
    int result = 0, rowhits, colhits[n];
    for (int i=0; i<m; i++) {
        for (int j=0; j<n; j++) {
            if (!j || grid[i][j-1] == 'W') {
                rowhits = 0;
                for (int k=j; k<n && grid[i][k] != 'W'; k++)
                    rowhits += grid[i][k] == 'E';
            }
            if (!i || grid[i-1][j] == 'W') {
                colhits[j] = 0;
                for (int k=i; k<m && grid[k][j] != 'W'; k++)
                    colhits[j] += grid[k][j] == 'E';
            }
            if (grid[i][j] == '0')
                result = max(result, rowhits + colhits[j]);
        }
    }
    return result;
}
{% endhighlight %}