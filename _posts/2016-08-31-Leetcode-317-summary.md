---
layout: post
title: Leetcode Problem 317 Summary
date: 2016-08-31
categories: blog
tags: [study]

---

# 题目

You want to build a house on an empty land which reaches all buildings in the shortest amount of distance. You can only move up, down, left and right. You are given a 2D grid of values 0, 1 or 2, where:

* Each 0 marks an empty land which you can pass by freely.
* Each 1 marks a building which you cannot pass through.
* Each 2 marks an obstacle which you cannot pass through.

For example, given three buildings at (0,0), (0,4), (2,2), and an obstacle at (0,2):

![](https://lisencn11.github.io/img/problem317.png)

The point (1,2) is an ideal empty land to build a house, as the total travel distance of 3+3+1=7 is minimal. So return 7.

Note:  
There will be at least one building. If it is not possible to build such house according to the above rules, return -1.

# 我的算法

我的算法是从每个 0 做 BFS 计算总和后比较。

discuss 的算法是从每个 1 做 BFS 途中对每个 0 结点做增量。

discuss 的效率更高，而且 discuss 还提供了一种处理边界的方法， shift 数组很值得学习。

# 代码

{% highlight java %}
public class Solution {
    public int shortestDistance(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][] sum = new int[m][n];
        int[][] visited = new int[m][n];
        
        int bldg = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                bldg = grid[i][j] == 1 ? bldg + 1 : bldg;
            }
        }
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 0) bfs(grid, sum, visited, i, j);
            }
        }
        int min = -1;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (visited[i][j] == bldg) {
                    if (min == -1) min = sum[i][j];
                    else min = sum[i][j] < min ? sum[i][j] : min;
                }
            }
        }
        return min;
    }
    
    /* sum is total distance from this point to all 1s
       visited is total 1s can reach from this point
       cache is points we have visited for this bfs*/
    private void bfs(int[][] grid, int[][] sum, int[][] visited, int row, int col) {
        Queue<int[]> queue = new LinkedList<>();
        queue.offer(new int[]{row, col});
        int m = grid.length, n = grid[0].length;
        int[][] cache = new int[m][n];
        cache[row][col] = 1;
        int distance = 0;
        int count = 0;
        int total = 0;
        while (!queue.isEmpty()) {
            int len = queue.size();
            for (int i = 0; i < len; i++) {
                int[] point = queue.poll();
                int x = point[0];
                int y = point[1];
                count = grid[x][y] == 1 ? count + 1 : count;
                total += grid[x][y] == 1 ? distance : 0;
                if (grid[x][y] == 1) continue;
                if (x > 0 && grid[x - 1][y] != 2 && cache[x - 1][y] == 0) {
                    queue.offer(new int[]{x - 1, y});
                    cache[x - 1][y] = 1;
                }
                if (x < m - 1 && grid[x + 1][y] != 2 && cache[x + 1][y] == 0) {
                    queue.offer(new int[]{x + 1, y});
                    cache[x + 1][y] = 1;
                }
                if (y > 0 && grid[x][y - 1] != 2 && cache[x][y - 1] == 0) {
                    queue.offer(new int[]{x, y - 1});
                    cache[x][y - 1] = 1;
                }
                if (y < n - 1 && grid[x][y + 1] != 2 && cache[x][y + 1] == 0) {
                    queue.offer(new int[]{x, y + 1});
                    cache[x][y + 1] = 1;
                }
            }
            distance++;
        }
        
        sum[row][col] = total;
        visited[row][col] = count;
    }
}
{% endhighlight %}

# discuss

{% highlight java %}
public class Solution {
    public int shortestDistance(int[][] grid) {
        if (grid == null || grid[0].length == 0) return 0;
        final int[] shift = new int[] {0, 1, 0, -1, 0};
        
        int row  = grid.length, col = grid[0].length;
        int[][] distance = new int[row][col];
        int[][] reach = new int[row][col];
        int buildingNum = 0;
        
        for (int i = 0; i < row; i++) {
            for (int j =0; j < col; j++) {
                if (grid[i][j] == 1) {
                    buildingNum++;
                    Queue<int[]> myQueue = new LinkedList<int[]>();
                    myQueue.offer(new int[] {i,j});

                    boolean[][] isVisited = new boolean[row][col];
                    int level = 1;
                    
                    while (!myQueue.isEmpty()) {
                        int qSize = myQueue.size();
                        for (int q = 0; q < qSize; q++) {
                            int[] curr = myQueue.poll();
                            
                            for (int k = 0; k < 4; k++) {
                                int nextRow = curr[0] + shift[k];
                                int nextCol = curr[1] + shift[k + 1];
                                
                                if (nextRow >= 0 && nextRow < row && nextCol >= 0 && nextCol < col
                                    && grid[nextRow][nextCol] == 0 && !isVisited[nextRow][nextCol]) {
                                        //The shortest distance from [nextRow][nextCol] to thic building
                                        // is 'level'.
                                        distance[nextRow][nextCol] += level;
                                        reach[nextRow][nextCol]++;
                                        
                                        isVisited[nextRow][nextCol] = true;
                                        myQueue.offer(new int[] {nextRow, nextCol});
                                    }
                            }
                        }
                        level++;
                    }
                }
            }
        }
        
        int shortest = Integer.MAX_VALUE;
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (grid[i][j] == 0 && reach[i][j] == buildingNum) {
                    shortest = Math.min(shortest, distance[i][j]);
                }
            }
        }
        
        return shortest == Integer.MAX_VALUE ? -1 : shortest;
        
        
    }
}
{% endhighlight %}