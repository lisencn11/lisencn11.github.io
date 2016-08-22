---
layout: post
title: Leetcode Problem 268 Summary
date: 2016-08-21
categories: blog
tags: [study]

---

# 题目

Given an array containing n distinct numbers taken from 0, 1, 2, ..., n, find the one that is missing from the array.

For example,  
Given nums = [0, 1, 3] return 2.

Note:  
Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?

# 我的算法

用异或操作做这道题，从 0 到 n 做一遍异或操作得到 distinct2 ，对 nums 数组整体做异或得到 distinct1，而 distinct1 和distinct2 异或的结果就是缺少的那个数。

# 代码

{% highlight java %}
public class Solution {
    public int missingNumber(int[] nums) {
        int distinct1 = 0;
        int distinct2 = 0;
        int len = nums.length;
        for (int i = 0; i < len; i++)
            distinct1 ^= nums[i];
        for (int i = 0; i <= len; i++) 
            distinct2 ^= i;
        
        return (distinct1 ^ distinct2);
    }
}
{% endhighlight %}