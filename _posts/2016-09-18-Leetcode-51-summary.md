---
layout: post
title: Leetcode Problem 51 Summary
date: 2016-09-18
categories: blog
tags: [study]

---

# 题目

The n-queens puzzle is the problem of placing n queens on an n×n chessboard such that no two queens attack each other.

![](https://lisencn11.github.io/img/problem51.png)

Given an integer n, return all distinct solutions to the n-queens puzzle.

Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space respectively.

For example,
There exist two distinct solutions to the 4-queens puzzle:

[  
 [".Q..",  // Solution 1  
  "...Q",  
  "Q...",  
  "..Q."],  

 ["..Q.",  // Solution 2  
  "Q...",  
  "...Q",  
  ".Q.."]  
]

# 我的算法

backtrack

# 代码

{% highlight java %}
public class Solution {
    public List<List<String>> solveNQueens(int n) {
        List<List<String>> result = new ArrayList<>();
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n; i++) {
            sb.append(".");
        }
        nQueensHelper(result, new ArrayList<>(), 0, n, sb.toString());
        return result;
    }
    
    private void nQueensHelper(List<List<String>> result, List<String> current, int row, int n, String dots) {
        if (row == n) {
            result.add(new ArrayList<>(current));
            return;
        }
        
        for (int i = 0; i < n; i++) {
            if (isValid(current, row, i, n)) {
                String valid = dots.substring(0, i) + "Q" + dots.substring(i + 1);
                current.add(valid);
                nQueensHelper(result, current, row + 1, n, dots);
                current.remove(current.size() - 1);
            }
        }
    }
    
    private boolean isValid(List<String> board, int i, int j, int n) {
        for (int k = 1; i - k >= 0; k++) {
            String row = board.get(i - k);
            if (row.charAt(j) == 'Q') return false;
            if (j + k < n && row.charAt(j + k) == 'Q') return false;
            if (j - k >= 0 && row.charAt(j - k) == 'Q') return false;
        }
        return true;
    }
}
{% endhighlight %}