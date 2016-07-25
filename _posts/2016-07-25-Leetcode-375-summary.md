---
layout: post
title: Leetcode Problem 375 Summary
date: 2016-07-25
categories: blog
tags: [study]

---

# 题目

输入一个整型 n ，从 1 到 n 中取一个数来猜，若猜错则要付出相应的罚款，如猜 5 ，猜错后罚 5 元，然后得到提示猜小了还是猜大了。题目要求我们需要多少钱来确保我们能够猜出这个数。

# 我的算法

第一思路是思考此题的最优策略是什么，我认为是对从 1 到 n 加和的一个二分查找，然而这个算法似乎并不对，我也无法证明这个算法的正确性。

首先理解题目，我们的目标是依照某个固定的策略进行猜数，这个策略猜数的最坏情况优于其它策略的最坏情况。

discuss 中使用的动态规划算法则是求出所有可能性算法，虽然我认为时间复杂度有点高，但是正确性确实显而易见。

priceForWrong[i][j] 代表从第 i 个数到第 j 个数这个区间，能够保证我们猜出数字的最少钱数，priceForWrong[1][n] 是我们所需求的最终结果，其他则为子问题。

{% highlight java %}
priceForWrong[i][j] = Math.min(priceForWrong[i][j], k + Math.max(k - 1 >= i ? priceForWrong[i][k - 1] : 0, k + 1 <= j ? priceForWrong[k + 1][j] : 0));
{% endhighlight %}

上面这个语句表示了动态规划的递推关系式。为了求 priceForWrong[i][j] ，我们假设我们取了从 i 到 j 的某个数字 k 作为第一个猜的数字，那么这样所需要的开销 = 从 i 到 k - 1 所需要的开销 (priceForWrong[i][k - 1]) + 从 k + 1 到 j 所需要的开销 (priceForWrong[k + 1][j]) + k 本身的开销 (k)。

通过前面关系式得到的结果是：加入我们对于 i 到 j 的子问题选择了 k 且选错，那么我们需要的最坏情况的开销（之所以 k 选错是因为我们计算的目标是至少需要多少钱，这是求最坏情况下我们需要多少钱）。而对于 priceForWrong[i][j] ，它的值是遍历从 i 到 j 的所有 k 后计算而得的最小值（即最坏情况下的最优解）。

# 代码

{% highlight java %}
public class Solution {
    public int getMoneyAmount(int n) {
        int[][] priceForWrong = new int[n + 1][n + 1];
        
        for (int step = 1; step < n; step++) {
            for (int i = 0; i + step <= n; i++) {
                int j = i + step;
                priceForWrong[i][j] = Integer.MAX_VALUE;
                for (int k = i; k <=j; k++) {
                    priceForWrong[i][j] = Math.min(priceForWrong[i][j], k + Math.max(k - 1 >= i ? priceForWrong[i][k - 1] : 0, k + 1 <= j ? priceForWrong[k + 1][j] : 0));
                }
            }
        }
        return priceForWrong[1][n];
    }
}
{% endhighlight %}