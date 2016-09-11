---
layout: post
title: Leetcode Problem 152 Summary
date: 2016-07-14
categories: blog
tags: [study]

---

# 题目

Find the contiguous subarray within an array (containing at least one number) which has the largest product.

For example, given the array [2,3,-2,4],
the contiguous subarray [2,3] has the largest product = 6.

# 我的算法

DP算法可以解决，类比最大和的DP算法，当前最大值为之前最大值加当前值或当前值本身。由于成绩涉及符号转换，我们同时需要纪录当前最小值，乘负数后当前最小值有可能转变为最大值。

# 代码

{% highlight java %}
public class Solution {
    public int maxProduct(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        
        int maxpre = nums[0];
        int minpre = nums[0];
        int max = nums[0];
        int maxhere = nums[0];
        int minhere = nums[0];
        
        for (int i = 1; i < nums.length; i++) {
            maxhere = Math.max(Math.max(maxpre * nums[i], minpre * nums[i]), nums[i]);
            minhere = Math.min(Math.min(maxpre * nums[i], minpre * nums[i]), nums[i]);
            max = Math.max(maxhere, max);
            maxpre = maxhere;
            minpre = minhere;
        }
        return max;
    }
}
{% endhighlight %}