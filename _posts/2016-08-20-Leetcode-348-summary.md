---
layout: post
title: Leetcode Problem 348 Summary
date: 2016-08-20
categories: blog
tags: [study]

---

# 题目

Design a Tic-tac-toe game that is played between two players on a n x n grid.

You may assume the following rules:

* A move is guaranteed to be valid and is placed on an empty block.
* Once a winning condition is reached, no more moves is allowed.
* A player who succeeds in placing n of their marks in a horizontal, vertical, or diagonal row wins the game.

Example:

![problem348](https://lisencn11.github.io/img/problem348.png)

Follow up:  
Could you do better than O(n2) per move() operation?

Hint:

Could you trade extra space such that move() operation can be done in O(1)?
You need two arrays: int rows[n], int cols[n], plus two variables: diagonal, anti_diagonal.

# 我的算法

要对 move 达到 O(1) 的时间复杂度，我们需要存储一些信息，比如当前行各玩家有多少棋子。

# 代码

{% highlight java %}
public class TicTacToe {
    int[][] board;
    int[][] horizontal;
    int[][] vertical;
    int[] diagonalLeft;
    int[] diagonalRight;
    int len;

    /** Initialize your data structure here. */
    public TicTacToe(int n) {
        board = new int[n][n];
        horizontal = new int[2][n];
        vertical = new int[2][n];
        diagonalLeft = new int[2];
        diagonalRight = new int[2];
        len = n;
    }
    
    /** Player {player} makes a move at ({row}, {col}).
        @param row The row of the board.
        @param col The column of the board.
        @param player The player, can be either 1 or 2.
        @return The current winning condition, can be either:
                0: No one wins.
                1: Player 1 wins.
                2: Player 2 wins. */
    public int move(int row, int col, int player) {
        board[row][col] = player;
        horizontal[player - 1][row]++;
        vertical[player - 1][col]++;
        diagonalLeft[player - 1] = row == col ? diagonalLeft[player - 1] + 1 : diagonalLeft[player - 1];
        diagonalRight[player - 1] = row + col == len - 1 ? diagonalRight[player - 1] + 1 : diagonalRight[player - 1];
        if (horizontal[player - 1][row] == len || vertical[player - 1][col] == len || diagonalLeft[player - 1] == len || diagonalRight[player - 1] == len) return player;
        else return 0;
    }
}

/**
 * Your TicTacToe object will be instantiated and called as such:
 * TicTacToe obj = new TicTacToe(n);
 * int param_1 = obj.move(row,col,player);
 */
{% endhighlight %}

# discuss

{% highlight java %}
public class TicTacToe {
    private int[] rows;
    private int[] cols;
    private int diagonal;
    private int antiDiagonal;
    
    /** Initialize your data structure here. */
    public TicTacToe(int n) {
        rows = new int[n];
        cols = new int[n];
    }
    
    /** Player {player} makes a move at ({row}, {col}).
        @param row The row of the board.
        @param col The column of the board.
        @param player The player, can be either 1 or 2.
        @return The current winning condition, can be either:
                0: No one wins.
                1: Player 1 wins.
                2: Player 2 wins. */
    public int move(int row, int col, int player) {
        int toAdd = player == 1 ? 1 : -1;
        
        rows[row] += toAdd;
        cols[col] += toAdd;
        if (row == col)
        {
            diagonal += toAdd;
        }
        
        if (col == (cols.length - row - 1))
        {
            antiDiagonal += toAdd;
        }
        
        int size = rows.length;
        if (Math.abs(rows[row]) == size ||
            Math.abs(cols[col]) == size ||
            Math.abs(diagonal) == size  ||
            Math.abs(antiDiagonal) == size)
        {
            return player;
        }
        
        return 0;
    }
}
{% endhighlight %}