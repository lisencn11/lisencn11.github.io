---
layout: post
title: Leetcode Problem 334 Summary
date: 2016-08-26
categories: blog
tags: [study]

---

# 题目

Given an unsorted array return whether an increasing subsequence of length 3 exists or not in the array.

Formally the function should:

>Return true if there exists i, j, k 
such that arr[i] < arr[j] < arr[k] given 0 ≤ i < j < k ≤ n-1 else return false.

Your algorithm should run in O(n) time complexity and O(1) space complexity.

Examples:  
Given [1, 2, 3, 4, 5],  
return true.

Given [5, 4, 3, 2, 1],  
return false.

# 我的算法

记录 first second，second 表示当前出现大于 second 的时候可以返回 true，first 记录当前最小值，表示当前出现大于 first 但是小于 second 的时候可以更新我们的 second。

# 代码

{% highlight java %}
public class Solution {
    public boolean increasingTriplet(int[] nums) {
        if (nums == null || nums.length < 3) return false;
        int first = Integer.MAX_VALUE;
        int second = Integer.MAX_VALUE;

        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > second) return true;
            if (nums[i] < first) first = nums[i];
            if (nums[i] > first) second = nums[i];
        }
        return false;
    }
}
{% endhighlight %}