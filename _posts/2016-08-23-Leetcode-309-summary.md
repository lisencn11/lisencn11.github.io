---
layout: post
title: Leetcode Problem 309 Summary
date: 2016-08-23
categories: blog
tags: [study]

---

# 题目

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times) with the following restrictions:

* You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
* After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)

Example:

prices = [1, 2, 3, 0, 2]  
maxProfit = 3  
transactions = [buy, sell, cooldown, buy, sell]

# 我的算法

动态规划算法，重点是找到递推公式。buy[i] 表示在以 buy sell 组成的交易序列中，截止第 i 天为止的序列以 buy 结尾，但不代表第 i 天为 buy，也可能没有进行操作，但是之前的操作是以 buy 结尾的。

# 代码

{% highlight java %}
public class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0) return 0;
        int[] buy = new int[prices.length + 2];
        int[] sell = new int[prices.length + 2];
        buy[0] = Integer.MIN_VALUE;
        sell[0] = 0;
        buy[1] = Integer.MIN_VALUE;
        sell[1] = 0;
        for (int i = 2; i < prices.length + 2; i++) {
            buy[i] = Math.max(sell[i - 2] - prices[i - 2], buy[i - 1]);
            sell[i] = Math.max(buy[i - 1] + prices[i - 2], sell[i - 1]);
        }
        
        return sell[sell.length - 1];
    }
}
{% endhighlight %}