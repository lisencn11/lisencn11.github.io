---
layout: post
title: Leetcode Problem 79 Summary
date: 2016-09-02
categories: blog
tags: [study]

---

# 题目

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

For example,

Given board =

[  
  ['A','B','C','E'],  
  ['S','F','C','S'],  
  ['A','D','E','E']  
]

word = "ABCCED", -> returns true,  
word = "SEE", -> returns true,  
word = "ABCB", -> returns false.

# 我的算法

DFS算法。

# 代码

{% highlight java %}
public class Solution {
    public boolean exist(char[][] board, String word) {
        int m = board.length, n = board[0].length;
        boolean[][] visited = new boolean[m][n];
        char[] data = word.toCharArray();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                visited[i][j] = false;
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (dfs(board, i, j, visited, data, 0)) return true;
            }
        }
        return false;
    }
    
    private boolean dfs(char[][] board, int row, int col, boolean[][] visited, char[] data, int index) {
        if (row < 0 || row >= board.length || col < 0 || col >= board[0].length) return false;
        if (visited[row][col]) return false;
        
        if (board[row][col] == data[index] && index < data.length - 1) {
            visited[row][col] = true;
            if (dfs(board, row - 1, col, visited, data, index + 1)) return true;
            if (dfs(board, row + 1, col, visited, data, index + 1)) return true;
            if (dfs(board, row, col - 1, visited, data, index + 1)) return true;
            if (dfs(board, row, col + 1, visited, data, index + 1)) return true;
            visited[row][col] = false;
        } else if (board[row][col] == data[index] && index == data.length - 1) {
            return true;
        }
        return false;
    }
}
{% endhighlight %}