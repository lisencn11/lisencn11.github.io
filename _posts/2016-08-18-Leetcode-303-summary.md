---
layout: post
title: Leetcode Problem 303 Summary
date: 2016-08-18
categories: blog
tags: [study]

---

# 题目

Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

Example:  
Given nums = [-2, 0, 3, -5, 2, -1]

sumRange(0, 2) -> 1  
sumRange(2, 5) -> -1  
sumRange(0, 5) -> -3

Note:  
You may assume that the array does not change.
There are many calls to sumRange function.

# 我的算法

因为会调用 sumRange() 很多次，所以考虑动态规划算法，将从 0 到 j 的和保存起来， 求从 i 到 j 的和只需要用 0 到 j 的和减去 0 到 i - 1 的和即可。

# 代码

{% highlight java %}
public class NumArray {
    int[] sums;

    public NumArray(int[] nums) {
        if (nums == null || nums.length == 0) {
            sums = nums;
            return;
        }
        
        sums = new int[nums.length];
        sums[0] = nums[0];
        for (int i = 1; i < nums.length; i++)
            sums[i] = nums[i] + sums[i - 1];
    }

    public int sumRange(int i, int j) {
        if (sums == null || sums.length == 0) return 0;
        
        if (i == 0) return sums[j];
        else return sums[j] - sums[i - 1];
    }
}


// Your NumArray object will be instantiated and called as such:
// NumArray numArray = new NumArray(nums);
// numArray.sumRange(0, 1);
// numArray.sumRange(1, 2);
{% endhighlight %}