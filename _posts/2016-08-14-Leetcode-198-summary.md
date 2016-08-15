---
layout: post
title: Leetcode Problem 198 Summary
date: 2016-08-14
categories: blog
tags: [study]

---

# 题目

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

# 我的算法

DP算法，对于截止第 n 家所能偷得的最大金钱为 f<sub>n</sub> = max{f<sub>n - 1</sub>, f<sub>n - 2</sub> + a<sub>n</sub>}

# 代码

{% highlight java %}
public class Solution {
    public int rob(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        if (nums.length == 1) return nums[0];

        int[] money = new int[nums.length];
        money[0] = nums[0];
        money[1] = Math.max(nums[0], nums[1]);
        for (int i = 2; i < nums.length; i++)
            money[i] = Math.max(money[i - 1], money[i - 2] + nums[i]);
        
        return money[money.length - 1];
    }
}
{% endhighlight %}