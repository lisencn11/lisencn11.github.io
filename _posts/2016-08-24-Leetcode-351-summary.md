---
layout: post
title: Leetcode Problem 351 Summary
date: 2016-08-24
categories: blog
tags: [study]

---

# 题目

Given an Android 3x3 key lock screen and two integers m and n, where 1 ≤ m ≤ n ≤ 9, count the total number of unlock patterns of the Android lock screen, which consist of minimum of m keys and maximum n keys.

Rules for a valid pattern:

1. Each pattern must connect at least m keys and at most n keys.
2. All the keys must be distinct.
3. If the line connecting two consecutive keys in the pattern passes through any other keys, the other keys must have previously selected in the pattern. No jumps through non selected key is allowed.
4. The order of keys used matters.

![](https://lisencn11.github.io/img/problem351.png)

Invalid move: 4 - 1 - 3 - 6   
Line 1 - 3 passes through key 2 which had not been selected in the pattern.

Invalid move: 4 - 1 - 9 - 2  
Line 1 - 9 passes through key 5 which had not been selected in the pattern.

Valid move: 2 - 4 - 1 - 3 - 6  
Line 1 - 3 is valid because it passes through key 2, which had been selected in the pattern

Valid move: 6 - 5 - 4 - 1 - 9 - 2  
Line 1 - 9 is valid because it passes through key 5, which had been selected in the pattern.

Example:  
Given m = 1, n = 1, return 9.

# 我的算法

递归算法 ＋ DFS，巧妙的一点是 1 3 7 9 四点对称，所以解法数量相同，直接乘 4 即可。

# 代码

{% highlight java %}
public class Solution {
    public int numberOfPatterns(int m, int n) {
        int skip[][] = new int[10][10];
        skip[1][3] = skip[3][1] = 2;
        skip[7][9] = skip[9][7] = 8;
        skip[1][7] = skip[7][1] = 4;
        skip[3][9] = skip[9][3] = 6;
        skip[1][9] = skip[9][1] = skip[2][8] = skip[8][2] = skip[3][7] = skip[7][3] = skip[4][6] = skip[6][4] = 5;
        boolean[] vis = new boolean[10];
        int count = 0;
        for (int i = m; i <= n; i++) {
            count += dfs(vis, skip, 1, i - 1) * 4;
            count += dfs(vis, skip, 2, i - 1) * 4;
            count += dfs(vis, skip, 5, i - 1);
        }
        
        return count;
    }
    
    private int dfs(boolean[] vis, int[][] skip, int cur, int remain) {
        if (remain < 0) return 0;
        if (remain == 0) return 1;
        vis[cur] = true;
        int count = 0;
        for (int i = 1; i <= 9; i++) {
            if (!vis[i] && (skip[cur][i] == 0 || vis[skip[cur][i]])) {
                count += dfs(vis, skip, i, remain - 1);
            }
        }
        vis[cur] = false;
        return count;
    }
}
{% endhighlight %}