---
layout: post
title: Leetcode Problem 97 Summary
date: 2016-09-15
categories: blog
tags: [study]

---

# 题目

Given s1, s2, s3, find whether s3 is formed by the interleaving of s1 and s2.

For example,  
Given:  
s1 = "aabcc",  
s2 = "dbbca",

When s3 = "aadbbcbcac", return true.  
When s3 = "aadbbbaccc", return false.

# 我的算法

DP算法，result[i][j]表示长度为i的s1和长度为j的s2能否构成长度为i+j的s3。

# 代码

{% highlight java %}
public class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        if (s1.length() + s2.length() != s3.length()) return false;
        
        boolean[][] result = new boolean[s1.length() + 1][s2.length() + 1];
        for (int i = 0; i <= s1.length(); i++) {
            for (int j = 0; j <= s2.length(); j++) {
                if (i == 0 && j == 0) {
                    result[i][j] = true;
                } else if (i == 0) {
                    result[i][j] = result[i][j - 1] && s2.charAt(j - 1) == s3.charAt(j - 1);
                } else if (j == 0) {
                    result[i][j] = result[i - 1][j] && s1.charAt(i - 1) == s3.charAt(i - 1);
                } else {
                    result[i][j] = ((result[i - 1][j] && s1.charAt(i - 1) == s3.charAt(i + j - 1)) || (result[i][j - 1] && s2.charAt(j - 1) == s3.charAt(i + j - 1)));
                }
            }
        }
        return result[s1.length()][s2.length()];
    }
}
{% endhighlight %}