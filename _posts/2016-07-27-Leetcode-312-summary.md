---
layout: post
title: Leetcode Problem 312 Summary
date: 2016-07-27
categories: blog
tags: [study]

---

# 题目

有 n 个气球，从 0 到 n - 1 编号，每一个气球 i 都一个分值 nums[i] 。你要刺破这些气球，刺破第 i 个气球可以得到的硬币数为 nums[left] * nums[i] * nums[right] ，即左右两个气球的分值和这个气球的乘积， i 被刺破后，其左右两边自然变成挨着，即若此时刺破左边的气球，其 nums[right] 为原 i 的 nums[right]。

求解最大能得到的硬币数。

1. 假设 nums[-1] = nums[n] = 1
2. 0 <= n <= 500 ， 0 <= nums[i] <= 100

# 我的算法

首先确定动态规划算法，或者待缓存的递归，若相求从 left 到 right 的最大硬币值，需要先刺破 i ，再对 i 左右两部分分别求最大值。这时我们发现一个问题，刺破 i 后，左右两部分变成相邻，于是我们无法分别独立计算他们的最大值。

discuss 中提供了 reverse thinking 的思路，即把 i 想成最后一个刺破的气球，如此一来左右两部分即可以单独计算了。假设我们要计算从 left 到 right 之间的气球最大硬币数（不含 left 和 right）那么最后刺破 i 时获得的硬币为 nums[left] * nums[i] * nums[right] ，左部分获得的硬币为 burst(left, i) ，右部分为 burst(i, right) 。这样就比较容易的得到了递推关系式。

# 代码

{% highlight java %}
public class Solution {
    public int maxCoins(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        
        int[] newNums = new int[nums.length + 2];
        int n = 1;
        for (int x : nums) if (x > 0) newNums[n++] = x;
        newNums[0] = newNums[n++] = 1;
        
        int[][] memo = new int[n][n];
        return burst(memo, newNums, 0, n - 1);
    }
    
    private int burst(int[][] memo, int[] newNums, int left, int right) {
        if (left + 1 == right) return 0;
        if (memo[left][right] > 0) return memo[left][right];
        
        int ans = 0;
        for (int i = left + 1; i < right; i++) {
            ans = Math.max(ans, newNums[left] * newNums[i] * newNums[right] + burst(memo, newNums, left, i) + burst(memo, newNums, i, right));
        }
        
        memo[left][right] = ans;
        return ans;
    }
}
{% endhighlight %}