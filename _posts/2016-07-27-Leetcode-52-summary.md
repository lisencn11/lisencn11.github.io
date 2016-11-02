---
layout: post
title: Leetcode Problem 52 Summary
date: 2016-07-27
categories: blog
tags: [study]

---

# 题目

Follow up for N-Queens problem.

Now, instead outputting board configurations, return the total number of distinct solutions.

# 我的算法

典型递归算法。

# 代码

{% highlight java %}
public class Solution {
    int solutions = 0;
    int[] board = null;
    
    public int totalNQueens(int n) {
        board = new int[n];
        solutions = 0;
        for (int i = 0; i < n; i++)
            board[i] = 0;
        nQueensHelper(0);
        return solutions;
    }
    
    private boolean isValid(int[] board, int row, int col) {
        for (int i = 0; i < row; i++)
            if (board[i] == col)
                return false;
        for (int i = row - 1, j = 1; i >= 0 && col - j >= 0; i--, j++)
            if (board[i] == col - j)
                return false;
        for (int i = row - 1, j = 1; i >= 0 && (col + j < board.length); i--, j++)
            if (board[i] == col + j)
                return false;
        return true;
    }
    
    private void nQueensHelper(int row) {
        if (board.length == row) {
            solutions++;
            return;
        }
        
        for (int col = 0; col < board.length; col++) {
            if (isValid(board, row, col)) {
                board[row] = col;
                nQueensHelper(row + 1);
            }
        }
    }
}
{% endhighlight %}

# 二刷

{% highlight java %}
public class Solution {
    public int totalNQueens(int n) {
        int[][] board = new int[n][n];
        int result = helper(board, 0, n);
        return result;
    }
    
    private int helper(int[][] board, int row, int n) {
        if (row == n) return 1;
        int result = 0;
        
        for (int i = 0; i < n; i++) {
            if (isValid(board, row, i)) {
                board[row][i] = 1;
                result += helper(board, row + 1, n);
                board[row][i] = 0;
            }
        }
        
        return result;
    }
    
    private boolean isValid(int[][] board, int row, int col) {
        for (int i = 1; row - i >= 0 && col - i >= 0; i++) {
            if (board[row - i][col - i] == 1) return false;
        }
        
        for (int i = 1; row - i >= 0 && col + i < board[0].length; i++) {
            if (board[row - i][col + i] == 1) return false;
        }
        
        for (int i = 1; row - i >= 0; i++) {
            if (board[row - i][col] == 1) return false;
        }
        
        return true;
    }
}
{% endhighlight %}