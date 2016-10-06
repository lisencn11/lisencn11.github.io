---
layout: post
title: Leetcode Problem 37 Summary
date: 2016-09-23
categories: blog
tags: [study]

---

# 题目

Write a program to solve a Sudoku puzzle by filling the empty cells.

Empty cells are indicated by the character '.'.

You may assume that there will be only one unique solution.

![](https://lisencn11.github.io/img/problem37_1.png)

![](https://lisencn11.github.io/img/problem37_2.png)

# 我的算法

backtrack + hashset

# 代码

{% highlight java %}
public class Solution {
    List<Set<Character>> rows;
    List<Set<Character>> cols;
    List<Set<Character>> sqrs;

    public void solveSudoku(char[][] board) {
        int m = board.length, n = board[0].length;
        rows = new ArrayList<>();
        cols = new ArrayList<>();
        sqrs = new ArrayList<>();
        for (int i = 0; i < 9; i++) {
            rows.add(new HashSet<>());
            cols.add(new HashSet<>());
            sqrs.add(new HashSet<>());
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] != '.') {
                    rows.get(i).add(board[i][j]);
                    cols.get(j).add(board[i][j]);
                    int sqrIndex = (i / 3) * 3 + (j / 3);
                    sqrs.get(sqrIndex).add(board[i][j]);
                }
            }
        }

        solveHelper(board, m, n, 0, 0);
    }
    
    private boolean solveHelper(char[][] board, int m, int n, int row, int col) {
        if (row == m) return true;
        if (col == n) return solveHelper(board, m, n, row + 1, 0);
        while (row < m) {
            while (col < n) {
                if (board[row][col] == '.') break;
                else col++;
            }
            if (col == n) {
                row++;
                col = 0;
            } else {
                break;
            }
        }
        if (row < m) {
            for (char i = '1'; i <= '9'; i++) {
                int index = (row / 3) * 3 + (col / 3);
                if (isValid(row, col, i)) {
                    rows.get(row).add(i);
                    cols.get(col).add(i);
                    sqrs.get(index).add(i);
                    board[row][col] = i;
                    if (solveHelper(board, m, n, row, col + 1)) return true;
                    else {
                        rows.get(row).remove(i);
                        cols.get(col).remove(i);
                        sqrs.get(index).remove(i);
                        board[row][col] = '.';
                    }
                }
            }
            return false;
        } else {
            return true;
        }
    }
    
    private boolean isValid(int row, int col, char target) {
        int index = (row / 3) * 3 + (col / 3);
        if (rows.get(row).contains(target)) return false;
        if (cols.get(col).contains(target)) return false;
        if (sqrs.get(index).contains(target)) return false;
        return true;
    }
}
{% endhighlight %}