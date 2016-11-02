---
layout: post
title: Leetcode Problem 419 Summary
date: 2016-10-17
categories: blog
tags: [study]

---

# 题目

Given an 2D board, count how many different battleships are in it. The battleships are represented with 'X's, empty slots are represented with '.'s. You may assume the following rules:

* You receive a valid board, made of only battleships or empty slots.
* Battleships can only be placed horizontally or vertically. In other words, they can only be made of the shape 1xN (1 row, N columns) or Nx1 (N rows, 1 column), where N can be of any size.
* At least one horizontal or vertical cell separates between two battleships - there are no adjacent battleships.

Example:

X..X
...X
...X

In the above board there are 2 battleships.

Invalid Example:

...X
XXXX
...X

This is not a valid board - as battleships will always have a cell separating between them.

Your algorithm should not modify the value of the board.

# 我的算法

看到一个为X的节点，就把这艘船的其他节点进行标记。

improve，其实并没有必要进行标记，因为我们只需要判断当前是否船的头节点，然后然后只对头节点计数即可。

# 代码

{% highlight java %}
public class Solution {
    private final int[] dx = {0, 1, 0, -1};
    private final int[] dy = {1, 0, -1, 0};
    
    public int countBattleships(char[][] board) {
        int m = board.length, n = board[0].length;
        boolean[][] checked = new boolean[m][n];
        int count = 0;
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                checked[i][j] = false;
            }
        }
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 'X' && !checked[i][j]) {
                    for (int k = 0; k < dx.length; k++) {
                        int x = i + dx[k];
                        int y = j + dy[k];
                        if (x >= 0 && y >= 0 && x < m && y < n && board[x][y] == 'X') {
                            if (dx[k] == 0) {
                                mark(board, checked, true, i, j);
                            } else {
                                mark(board, checked, false, i, j);
                            }
                        }
                    }
                    count++;
                }
            }
        }
        
        return count;
    }
    
    private void mark(char[][] board, boolean[][] checked, boolean horizon, int i, int j) {
        if (horizon) {
            int index = j;
            while (index < board[0].length && board[i][index] == 'X') {
                checked[i][index] = true;
                index++;
            }
            index = j;
            while (index >= 0 && board[i][index] == 'X') {
                checked[i][index] = true;
                index--;
            }
        } else {
            int index = i;
            while (index < board.length && board[index][j] == 'X') {
                checked[index][j] = true;
                index++;
            }
            index = i;
            while (index >= 0 && board[index][j] == 'X') {
                checked[index][j] = true;
                index--;
            }
        }
    }
}
{% endhighlight %}