---
layout: post
title: Leetcode Problem 283 Summary
date: 2016-08-12
categories: blog
tags: [study]

---

# 题目

Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

For example, given nums = [0, 1, 0, 3, 12], after calling your function, nums should be [1, 3, 12, 0, 0].

Note:  
1. You must do this in-place without making a copy of the array.  
2. Minimize the total number of operations.

# 我的算法

用两个pointer，第一个指向当前序列中第一个 0 ，第二个指向这个 0 后的第一个非 0 ，然后将这两个数对调。

# 代码

{% highlight java %}
public class Solution {
    public void moveZeroes(int[] nums) {
        if (nums == null || nums.length == 0) return;
        
        int p1 = 0, p2 = 0;
        while (p1 < nums.length) {
            while (p2 < nums.length && nums[p2] != 0) p2++;
            while (p1 < nums.length && (nums[p1] == 0 || p1 < p2)) p1++;
            if (p1 == nums.length) return;
            nums[p2] = nums[p1];
            nums[p1] = 0;
        }
    }
}
{% endhighlight %}