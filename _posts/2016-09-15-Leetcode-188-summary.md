---
layout: post
title: Leetcode Problem 188 Summary
date: 2016-09-15
categories: blog
tags: [study]

---

# 题目

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete at most k transactions.

Note:  
You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

# 我的算法

和123题思路相同，但是多了一个quickSolve的方法用于处理大k的情况。

# 代码

{% highlight java %}
public class Solution {
    public int maxProfit(int k, int[] prices) {
        if (prices == null || prices.length == 0) return 0;
        int n = prices.length;
        if (k >= n / 2) return quickSolve(prices);
        int[] preResult = new int[n];
        
        for (int i = 1; i <= k; i++) {
            int tempMax = preResult[0] - prices[0];
            int[] result = new int[n];
            for (int j = 1; j < n; j++) {
                result[j] = Math.max(result[j - 1], prices[j] + tempMax);
                tempMax = Math.max(tempMax, preResult[j - 1] - prices[j]);
            }
            preResult = result;
        }
        return preResult[n - 1];
    }
    
    private int quickSolve(int[] prices) {
        int sum = 0;
        for (int i = 1; i < prices.length; i++) {
            if (prices[i] > prices[i - 1]) {
                sum += prices[i] - prices[i - 1];
            }
        }
        return sum;
    }
}
{% endhighlight %}