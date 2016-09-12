---
layout: post
title: Leetcode Problem 72 Summary
date: 2016-09-11
categories: blog
tags: [study]

---

# 题目

Given two words word1 and word2, find the minimum number of steps required to convert word1 to word2. (each operation is counted as 1 step.)

You have the following 3 operations permitted on a word:

a) Insert a character  
b) Delete a character  
c) Replace a character

# 我的算法

DP 动态规划

用数组 trans[i][j] 表示将 word1 从 1 到 i 位 转化为 word2 从 1 到 j 位所需要的最少操作。

如果 word1[i] == word2[j] 那么我们有三种可能操作，分别是：

1. 将 word1 从 0 到 i 转化为 word2 的 0 到 j - 1 ，然后 insert word2 最后一位
2. 将 word1 从 0 到 i - 1 转化为 word2 的 0 到 j ，然后 delete word1 最后一位
3. 将 word1 从 0 到 i - 1 转化为 word2 的 0 到 j - 1 ，然后保留最后不变

如果 word1[i] != word2[j] 那么我们对应上面的三种可能的操作，只需要将第 3 种改为 replace 最后一位即可，求三种操作的最小结果即可。

# 代码

{% highlight java %}
public class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length(), n = word2.length();
        int[][] trans = new int[m + 1][n + 1];
        for (int i = 0; i < m + 1; i++) {
            trans[i][0] = i;
        }
        for (int i = 0; i < n + 1; i++) {
            trans[0][i] = i;
        }
        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < n + 1; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    trans[i][j] = Math.min(Math.min(trans[i - 1][j] + 1, trans[i - 1][j - 1]), trans[i][j - 1] + 1);
                } else {
                    trans[i][j] = Math.min(Math.min(trans[i][j - 1] + 1, trans[i - 1][j] + 1), trans[i - 1][j - 1] + 1);
                }
            }
        }
        return trans[m][n];
    }
}
{% endhighlight %}