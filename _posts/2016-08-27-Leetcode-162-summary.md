---
layout: post
title: Leetcode Problem 162 Summary
date: 2016-08-27
categories: blog
tags: [study]

---

# 题目

A peak element is an element that is greater than its neighbors.

Given an input array where num[i] ≠ num[i+1], find a peak element and return its index.

The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

You may imagine that num[-1] = num[n] = -∞.

For example, in array [1, 2, 3, 1], 3 is a peak element and your function should return the index number 2.

Note:  
Your solution should be in logarithmic complexity.

# 我的算法

一个比较重要的条件是 index 为 -1 和 n 的值为负无穷，利用这个条件，我们使用二分查找，对于 mid 的值如果大于 mid + 1 的值，那么证明在 0 到 mid 间至少有一个 peak。反之小于就是在 mid + 1 到 end 间有一个 peak。

# 代码

{% highlight java %}
public class Solution {
    public int findPeakElement(int[] nums) {
        return helper(nums, 0, nums.length - 1);
    }
    
    private int helper(int[] nums, int start, int end) {
        if (start == end) return start;
        
        int mid = start + (end - start) / 2;
        int midNext = mid + 1;
        if (nums[mid] > nums[midNext]) return helper(nums, start, mid);
        else return helper(nums, midNext, end);
    }
}
{% endhighlight %}