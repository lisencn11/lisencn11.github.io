---
layout: post
title: Leetcode Problem 52 Summary
date: 2016-07-27
categories: blog
tags: [study]

---

# 题目

**输入**一个整型 n ，表示 n queens 问题中的皇后数量

**输出**所有可能的皇后摆放的数量。

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