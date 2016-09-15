---
layout: post
title: Leetcode Problem 123 Summary
date: 2016-09-14
categories: blog
tags: [study]

---

# 题目

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete at most two transactions.

Note:  
You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

# 我的算法

DP。 详细解释在[这里](https://discuss.leetcode.com/topic/4766/a-clean-dp-solution-which-generalizes-to-k-transactions/2)

# 代码

{% highlight java %}
public class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length <= 1) return 0;
        int[][] profits = new int[3][prices.length];
        
        for (int i = 1; i <= 2; i++) {
            int tempMax = profits[i - 1][0] - prices[0];
            for (int j = 1; j < prices.length; j++) {
                profits[i][j] = Math.max(profits[i][j - 1], tempMax + prices[j]);
                tempMax = Math.max(tempMax, profits[i - 1][j] - prices[j]);
            }
        }
        return profits[2][prices.length - 1];
    }
}
{% endhighlight %}