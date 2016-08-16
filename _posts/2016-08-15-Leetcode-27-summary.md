---
layout: post
title: Leetcode Problem 27 Summary
date: 2016-08-15
categories: blog
tags: [study]

---

# 题目

Given an array and a value, remove all instances of that value in place and return the new length.

Do not allocate extra space for another array, you must do this in place with constant memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

Example:
Given input array nums = [3,2,2,3], val = 3

Your function should return length = 2, with the first two elements of nums being 2.

# 我的算法

two pointers 一个从前向后指向第一个 target ，一个从后向前指向第一个非 target ，交换。

# 代码

{% highlight java %}
public class Solution {
    public int removeElement(int[] nums, int val) {
        if (nums == null || nums.length == 0) return 0;
        
        int p1 = 0;
        int p2 = nums.length - 1;
        while (p1 < p2) {
            while (p1 < p2 && nums[p1] != val) p1++;
            while (p1 < p2 && nums[p2] == val) p2--;
            int temp = nums[p1];
            nums[p1] = nums[p2];
            nums[p2] = temp;
        }
        
        return nums[p1] == val ? p1 : p1 + 1;
    }
}
{% endhighlight %}