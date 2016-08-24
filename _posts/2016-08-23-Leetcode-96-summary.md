---
layout: post
title: Leetcode Problem 96 Summary
date: 2016-08-23
categories: blog
tags: [study]

---

# 题目

Given n, how many structurally unique BST's (binary search trees) that store values 1...n?

For example,  
Given n = 3, there are a total of 5 unique BST's.

![](https://lisencn11.github.io/img/problem96.png)

# 我的算法

动态规划和递归相结合的算法，每个结点数都对应着一个解，而对于 n 个结点数，解等于假设从 1 到 n 分别为根结点，左右子树的解法的乘积。

# 代码

{% highlight java %}
public class Solution {
    int[] ways;
    public int numTrees(int n) {
        ways = new int[n + 1];
        return helper(n);
    }
    
    private int helper(int n) {
        if (n == 0) return 0;
        if (n == 1) return 1;
        if (ways[n] != 0) return ways[n];
        
        int count = 0;
        for (int i = 1; i <= n; i++) {
            int left = i - 1;
            int right = n - i;
            int countLeft = helper(left);
            int countRight = helper(right);
            if (countLeft == 0 || countRight == 0) {
                count += countLeft == 0 ? countRight : countLeft;
            } else {
                count += countLeft * countRight;
            }
        }
        ways[n] = count;
        
        return count;
    }
}
{% endhighlight %}

# discuss

discuss 中有无须递归的方法，[这里](https://discuss.leetcode.com/topic/8398/dp-solution-in-6-lines-with-explanation-f-i-n-g-i-1-g-n-i/20)