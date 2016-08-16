---
layout: post
title: Leetcode Problem 257 Summary
date: 2016-08-16
categories: blog
tags: [study]

---

# 题目

There is a fence with n posts, each post can be painted with one of the k colors.

You have to paint all the posts such that no more than two adjacent fence posts have the same color.

Return the total number of ways you can paint the fence.

Note:  
n and k are non-negative integers.

# 我的算法

充满歧义的一道题，首先fence我最开始认为围墙肯定是围成一圈的，后来发现并不用，其次对颜色的要求是最多只能有连续两个柱子有相同颜色。

采用DP算法，solution[i]表示第i根柱子的最多可能性，solution[i] = solution[i - 2] * (k - 1) + solution[i - 1] * (k - 1)表示第i根柱子与前一根柱子颜色相同的可能性加上与前面颜色不同的可能性。

# 代码

{% highlight java %}
public class Solution {
    public int numWays(int n, int k) {
        if (n == 0) return 0;
        if (n == 1) return k;

        int[] solution = new int[n + 1];
        solution[1] = k;
        solution[2] = k * k;
        for (int i = 3; i <= n; i++) {
            solution[i] = solution[i - 2] * (k - 1) + solution[i - 1] * (k - 1);
        }
        
        return solution[n];
    }
}
{% endhighlight %}