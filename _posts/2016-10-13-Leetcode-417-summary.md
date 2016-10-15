---
layout: post
title: Leetcode Problem 417 Summary
date: 2016-10-13
categories: blog
tags: [study]

---

# 题目

Given an m x n matrix of non-negative integers representing the height of each unit cell in a continent, the "Pacific ocean" touches the left and top edges of the matrix and the "Atlantic ocean" touches the right and bottom edges.

Water can only flow in four directions (up, down, left, or right) from a cell to another one with height equal or lower.

Find the list of grid coordinates where water can flow to both the Pacific and Atlantic ocean.

Note:

1. The order of returned grid coordinates does not matter.
2. Both m and n are less than 150.

Example:

Given the following 5x5 matrix:

![](https://lisencn11.github.io/img/problem417.png)

Return:

[0, 4], [1, 3], [1, 4], [2, 2], [3, 0], [3, 1], [4, 0] \(positions with parentheses in above matrix).

# 我的算法

从岸边的各个节点开始做BFS，求出matrix中能够流到pacific的点和能够流到atlantic的点，再求交集。

# 代码

{% highlight java %}
import java.awt.Point;
public class Solution {
    public List<int[]> pacificAtlantic(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return new ArrayList<>();
        
        int[] dx = {0, 1, 0, -1};
        int[] dy = {1, 0, -1, 0};
        int m = matrix.length, n = matrix[0].length;
        boolean[][] pacific = new boolean[m][n];
        boolean[][] atlantic = new boolean[m][n];
        ArrayList<int[]> result = new ArrayList<>();
        Queue<int[]> pQueue = new LinkedList<>();
        Queue<int[]> aQueue = new LinkedList<>();
        
        for (int i = 0; i < m; i++) {
            pQueue.offer(new int[]{i, 0});
            aQueue.offer(new int[]{i, n - 1});
            pacific[i][0] = true;
            atlantic[i][n - 1] = true;
        }
        
        for (int j = 0; j < n; j++) {
            pQueue.offer(new int[]{0, j});
            aQueue.offer(new int[]{m - 1, j});
            pacific[0][j] = true;
            atlantic[m - 1][j] = true;
        }
        
        while (!pQueue.isEmpty()) {
            int[] pos = pQueue.poll();
            for (int i = 0; i < 4; i++) {
                int x = pos[0] + dx[i];
                int y = pos[1] + dy[i];
                if (x >= 0 && x < m && y >= 0 && y < n && !pacific[x][y] && matrix[x][y] >= matrix[pos[0]][pos[1]]) {
                    pQueue.offer(new int[]{x, y});
                    pacific[x][y] = true;
                }
            }
        }
        
        while (!aQueue.isEmpty()) {
            int[] pos = aQueue.poll();
            for (int i = 0; i < 4; i++) {
                int x = pos[0] + dx[i];
                int y = pos[1] + dy[i];
                if (x >= 0 && x < m && y >= 0 && y < n && !atlantic[x][y] && matrix[x][y] >= matrix[pos[0]][pos[1]]) {
                    aQueue.offer(new int[]{x, y});
                    atlantic[x][y] = true;
                }
            }
        }
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (pacific[i][j] && atlantic[i][j]) {
                    result.add(new int[]{i, j});
                }
            }
        }
        
        return result;
    }
}
{% endhighlight %}