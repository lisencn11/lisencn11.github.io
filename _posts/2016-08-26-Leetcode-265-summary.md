---
layout: post
title: Leetcode Problem 265 Summary
date: 2016-08-26
categories: blog
tags: [study]

---

# 题目

There are a row of n houses, each house can be painted with one of the k colors. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a n x k cost matrix. For example, costs[0][0] is the cost of painting house 0 with color 0; costs[1][2] is the cost of painting house 1 with color 2, and so on... Find the minimum cost to paint all houses.

Note:  
All costs are positive integers.

Follow up:  
Could you solve it in O(nk) runtime?

# 我的算法

DP 问题，记录前一个房子的各种颜色最小开销，当前房子的开销等于前一个房子所有颜色中最小开销加上当前颜色开销，除了对应当前颜色 index 等于前一个前一个最小的，这个颜色加上前一个房子次小颜色的开销。

discuss 比起我的代码简洁不少，值得学习。

# 代码

{% highlight java %}
public class Solution {
    public int minCostII(int[][] costs) {
        if (costs == null || costs.length == 0) return 0;
        
        int n = costs.length, k = costs[0].length;
        int min1 = Integer.MAX_VALUE;
        int min2 = Integer.MAX_VALUE;
        int min1Index = 0;
        int min2Index = 0;
        int[] prev = new int[k];
        for (int i = 0; i < k; i++) {
            prev[i] = costs[0][i];
            if (prev[i] < min2) {
                min2Index = i;
                min2 = prev[i];
                if (min2 < min1) {
                    int temp = min1;
                    min1 = min2;
                    min2 = temp;
                    temp = min1Index;
                    min1Index = min2Index;
                    min2Index = temp;
                }
            }
        }
        
        for (int i = 1; i < n; i++) {
            int prevMin1 = min1Index;
            int prevMin2 = min2Index;
            int least = prev[prevMin1];
            int less = prev[prevMin2];
            min1 = Integer.MAX_VALUE;
            min2 = Integer.MAX_VALUE;
            min1Index = 0;
            min2Index = 0;
            for (int j = 0; j < k; j++) {
                if (j == prevMin1) prev[j] = less + costs[i][j];
                else prev[j] = least + costs[i][j];
                if (prev[j] < min2) {
                    min2Index = j;
                    min2 = prev[j];
                    if (min2 < min1) {
                        int temp = min1;
                        min1 = min2;
                        min2 = temp;
                        temp = min1Index;
                        min1Index = min2Index;
                        min2Index = temp;
                    }
                }
            }
        }
        
        return prev[min1Index];
    }
}
{% endhighlight %}

# discuss

{% highlight java %}
public int minCostII(int[][] costs) {
    if (costs == null || costs.length == 0) return 0;
        
    int n = costs.length, k = costs[0].length;
    // min1 is the index of the 1st-smallest cost till previous house
    // min2 is the index of the 2nd-smallest cost till previous house
    int min1 = -1, min2 = -1;
    
    for (int i = 0; i < n; i++) {
        int last1 = min1, last2 = min2;
        min1 = -1; min2 = -1;
        
        for (int j = 0; j < k; j++) {
            if (j != last1) {
                // current color j is different to last min1
                costs[i][j] += last1 < 0 ? 0 : costs[i - 1][last1];
            } else {
                costs[i][j] += last2 < 0 ? 0 : costs[i - 1][last2];
            }
            
            // find the indices of 1st and 2nd smallest cost of painting current house i
            if (min1 < 0 || costs[i][j] < costs[i][min1]) {
                min2 = min1; min1 = j;
            } else if (min2 < 0 || costs[i][j] < costs[i][min2]) {
                min2 = j;
            }
        }
    }
    
    return costs[n - 1][min1];
}
{% endhighlight %}