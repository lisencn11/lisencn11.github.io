---
layout: post
title: Leetcode Problem 276 Summary
date: 2016-08-16
categories: blog
tags: [study]

---

# 题目

Determine if a Sudoku is valid, according to: Sudoku Puzzles - The Rules.

The Sudoku board could be partially filled, where empty cells are filled with the character '.'.

![sudoku](http://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

A valid Sudoku board (partially filled) is not necessarily solvable. Only the filled cells need to be validated.

# 我的算法

这道题仅仅考察数组和哈希表的使用，没有涉及算法。

# 代码

{% highlight java %}
public class Solution {
    public boolean isValidSudoku(char[][] board) {
        for (int i = 0; i < board.length; i++) {
            if (!isValid(board, i, 0, i + 1, board[0].length)) return false;
        }
        for (int i = 0; i < board[0].length; i++) {
            if (!isValid(board, 0, i, board.length, i + 1)) return false;
        }
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (!isValid(board, 3 * i, 3 * j, 3 * (i + 1), 3 * (j + 1))) return false;
            }
        }
        
        return true;
    }
    
    private boolean isValid(char[][] board, int x1, int y1, int x2, int y2) {
        // System.out.println(x1 + " " + y1 + " " + x2 + " " + y2);
        Set<Character> set = new HashSet<>();
        for (int i = x1; i < x2; i++) {
            for (int j = y1; j < y2; j++) {
                if (board[i][j] != '.' && !set.add(board[i][j])) return false;
            }
        }
        return true;
    }
}
{% endhighlight %}