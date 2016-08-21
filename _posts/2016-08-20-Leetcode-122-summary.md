---
layout: post
title: Leetcode Problem 122 Summary
date: 2016-08-20
categories: blog
tags: [study]

---

# 题目

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). However, you may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

# 我的算法

考察贪心算法，我们总是在最低点买，最高点卖。

discuss 中不考虑买卖策略，直接计算我们获得所有上升阶段的 profit ，我觉得是非常好的算法。

# 代码

{% highlight java %}
public class Solution {
    public int maxProfit(int[] prices) {
        int buy = -1;
        int profit = 0;
        
        for (int i = 0; i < prices.length - 1; i++) {
            if (buy == -1 && prices[i] < prices[i + 1]) {
                buy = prices[i];
            } else if (buy != -1 && prices[i] > prices[i + 1]) {
                profit += prices[i] - buy;
                buy = -1;
            }
        }
        
        if (buy != -1 && prices[prices.length - 1] > buy) {
            profit += prices[prices.length - 1] - buy;
        }
        
        return profit;
    }
}
{% endhighlight %}

# discuss

{% highlight java %}
public int maxProfit(int[] prices) {
    int total = 0;
    for (int i=0; i< prices.length-1; i++) {
        if (prices[i+1]>prices[i]) total += prices[i+1]-prices[i];
    }
    
    return total;
}
{% endhighlight %}