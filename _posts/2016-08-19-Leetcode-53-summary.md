---
layout: post
title: Leetcode Problem 53 Summary
date: 2016-08-19
categories: blog
tags: [study]

---

# 题目

Find the contiguous subarray within an array (containing at least one number) which has the largest sum.

For example, given the array [-2,1,-3,4,-1,2,1,-5,4],
the contiguous subarray [4,-1,2,1] has the largest sum = 6.

# 我的算法

考察动态规划，当前加和取决于 **之前加和和当前元素的和** 以及 **当前元素本身** 的大小比较

# 代码

{% highlight java %}
public class Solution {
    public int maxSubArray(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        int sum = 0;
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > sum + nums[i]) sum = nums[i];
            else sum = sum + nums[i];
            max = sum > max ? sum : max;
        }
        return max;
    }
}
{% endhighlight %}