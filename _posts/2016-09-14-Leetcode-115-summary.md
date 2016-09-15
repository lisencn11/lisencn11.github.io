---
layout: post
title: Leetcode Problem 115 Summary
date: 2016-09-14
categories: blog
tags: [study]

---

# 题目

Given a string S and a string T, count the number of distinct subsequences of T in S.

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, "ACE" is a subsequence of "ABCDE" while "AEC" is not).

Here is an example:  
S = "rabbbit", T = "rabbit"

Return 3.

# 我的算法

DP 算法典型子串问题，解法通常是用一个 mn 的矩阵，横纵坐标分别代表字符串来解。result[i][j] 表示 t 子串从 0 到 i，s 子串从 0 到 j 的子串组合数，result[i][j] = result[i - 1][j - 1] + result[i][j - 1] 表示当： t 的 i 字符 和 s 的 j 字符相等时，子串可能数为 t(0..i-1) 和 s(0...j-1) 的解加上 t(0...i) 和 s(0...j-1)的解。

# 代码

{% highlight java %}
public class Solution {
    public int numDistinct(String s, String t) {
        int m = s.length(), n = t.length();
        int[][] result = new int[n + 1][m + 1];
        for (int i = 0; i <= m; i++) {
            result[0][i] = 1;
        }
        
        for (int i = 1; i <= n; i++) {
            int indexT = i - 1;
            for (int j = i; j <= m; j++) {
                int indexS = j - 1;
                if (t.charAt(indexT) == s.charAt(indexS)) {
                    result[i][j] = result[i - 1][j - 1] + result[i][j - 1];
                } else {
                    result[i][j] = result[i][j - 1];
                }
            }
        }
        return result[n][m];
    }
}
{% endhighlight %}