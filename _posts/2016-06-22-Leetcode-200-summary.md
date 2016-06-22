---
layout: post
title: Leetcode Problem 200 Summary
date: 2016-06-22
categories: blog
tags: [study]

---

# 题目

**输入**一个二维数组char[][]，其中包含1和0，0表示水，1表示陆地，上下左右表示接邻，所有接邻的陆地构成一个岛，设计算法求二维数组中有几个岛。（二维数组视为被水包围，即最外围全为0）

**输出**岛的个数

输入：  
11110  
11010  
11000  
00000

输出：1 

# 我的算法

### 初步思路

第一眼看这题，想到动态规划：从头到尾遍历二维数组，如果当前cell为'1'，且左边或者上边存在1，则表示同一片岛屿，否则岛屿数＋1。

问题：反例：  
111  
010  
111  
没有考虑周全，不仅仅是cell的左边和上边对岛屿数有影响。

### 改良思路

图的遍历算法，使用深度优先或广度优先皆可，从头到尾遍历二维数组，如果当前cell为'1'，则以当前cell为起点，广度优先遍历上下左右四个节点，将遍历到的节点置为'0'，这样可以保证没有遗漏。为了减少边界条件，新声明一个二维数组newGrid[i+2][j+2]，将最外围置为'0'。

### 代码

注：广度优先使用的Queue中的参数原本声明的是Point类，但是似乎leetcode不支持，于是改为两个Queue，会在一定程度上影响效率。

```java
public class Solution {
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        int res = 0;
        Queue<Integer> xNode = null;
        Queue<Integer> yNode = null;
        char[][] newGrid = new char[grid.length + 2][grid[0].length + 2];
        
        for (int i = 0; i < newGrid.length; i++) {
            for (int j = 0; j < newGrid[0].length; j++) {
                newGrid[i][j] = '0';
            }
        }
        
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                newGrid[i + 1][j + 1] = grid[i][j];
            }
        }
        
        for (int i = 0; i < newGrid.length; i++) {
            for (int j = 0; j < newGrid[0].length; j++) {
                if (newGrid[i][j] == '1') {
                    xNode = new LinkedList<Integer>();
                    yNode = new LinkedList<Integer>();
                    xNode.offer(i);
                    yNode.offer(j);
                    while (!xNode.isEmpty()) {
                        int x = xNode.poll();
                        int y = yNode.poll();
                        if (newGrid[x - 1][y] == '1') {
                            xNode.offer(x - 1);
                            yNode.offer(y);
                        }
                        if (newGrid[x][y - 1] == '1') {
                            xNode.offer(x);
                            yNode.offer(y - 1);
                        }
                        if (newGrid[x + 1][y] == '1') {
                            xNode.offer(x + 1);
                            yNode.offer(y);
                        }
                        if (newGrid[x][y + 1] == '1') {
                            xNode.offer(x);
                            yNode.offer(y + 1);
                        }
                        newGrid[x][y] = '0'; // 问题所在
                    }
                    res++;
                }
            }
        }
        
        return res;
    }
}
```

问题：测试用例给了一个17*21的矩阵后出现超时。

### 进一步改良

观察发现算法存在一个小问题，即是在每次从Queue中取出后才置'0'，这样会导致很多cell重复放入Queue，导致超时。改良的简单方法是在每次放入Queue的同时置'0'，这样保证所有cell只会被放入Queue中一次，算法复杂度为O(n)。

# 代码

```java
public class Solution {
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        int res = 0;
        Queue<Integer> xNode = null;
        Queue<Integer> yNode = null;
        char[][] newGrid = new char[grid.length + 2][grid[0].length + 2];
        
        for (int i = 0; i < newGrid.length; i++) {
            for (int j = 0; j < newGrid[0].length; j++) {
                newGrid[i][j] = '0';
            }
        }
        
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                newGrid[i + 1][j + 1] = grid[i][j];
            }
        }
        
        for (int i = 0; i < newGrid.length; i++) {
            for (int j = 0; j < newGrid[0].length; j++) {
                if (newGrid[i][j] == '1') {
                    xNode = new LinkedList<Integer>();
                    yNode = new LinkedList<Integer>();
                    xNode.offer(i);
                    yNode.offer(j);
                    newGrid[i][j] = '0';
                    while (!xNode.isEmpty()) {
                        int x = xNode.poll();
                        int y = yNode.poll();
                        if (newGrid[x - 1][y] == '1') {
                            xNode.offer(x - 1);
                            yNode.offer(y);
                            newGrid[x - 1][y] = '0';
                        }
                        if (newGrid[x][y - 1] == '1') {
                            xNode.offer(x);
                            yNode.offer(y - 1);
                            newGrid[x][y - 1] = '0';
                        }
                        if (newGrid[x + 1][y] == '1') {
                            xNode.offer(x + 1);
                            yNode.offer(y);
                            newGrid[x + 1][y] = '0';
                        }
                        if (newGrid[x][y + 1] == '1') {
                            xNode.offer(x);
                            yNode.offer(y + 1);
                            newGrid[x][y + 1] = '0';
                        }
                    }
                    res++;
                }
            }
        }
        
        return res;
    }
}
```
