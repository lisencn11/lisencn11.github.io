---
layout: post
title: Leetcode Problem 322 Summary
date: 2016-07-09
categories: blog
tags: [study]

---

# 题目

**输入**一个数组，包含无限量的不同币值；一个目标值。

**输出**如果能够用所给币值配除目标值则返回所需硬币数，否则返回－1。

如：  
>coins = [1, 2, 5], amount = 11  
return 3 (11 = 5 + 5 + 1)

>coins = [2], amount = 3  
return -1

# 我的算法

典型DP问题，用一个reachable数组存储所需硬币数，对于reachable[i]，用 i 分别减去各币值看是否有解，并选取最小值。

# 代码

{% highlight java %}
public class Solution {
    public int coinChange(int[] coins, int amount) {
        if (coins == null || coins.length < 0 || amount < 0) return -1;
        
        int[] reachable = new int[amount + 1];
        reachable[0] = 0;
        for (int i = 1; i <= amount; i++)
            reachable[i] = -1;
            
        for (int i = 1; i <= amount; i++) {
            int min = -1;
            for (int j = 0; j < coins.length; j++) {
                if ((i - coins[j] >= 0) && (reachable[i - coins[j]] > -1) && (reachable[i - coins[j]] < min || min == -1)) {
                    min = reachable[i - coins[j]] + 1;
                }
            }
            reachable[i] = min;
        }
        
        return reachable[amount];      
    }
}
{% endhighlight %}