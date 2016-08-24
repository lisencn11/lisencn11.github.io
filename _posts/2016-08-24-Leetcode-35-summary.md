---
layout: post
title: Leetcode Problem 35 Summary
date: 2016-08-24
categories: blog
tags: [study]

---

# 题目

Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

Here are few examples.  
[1,3,5,6], 5 → 2  
[1,3,5,6], 2 → 1  
[1,3,5,6], 7 → 4  
[1,3,5,6], 0 → 0

# 我的算法

二分查找，注意边界条件，以后写完代码要自己设计测例尽兴测试。

# 代码

{% highlight java %}
public class Solution {
    public int searchInsert(int[] nums, int target) {
        if (target > nums[nums.length - 1]) return nums.length;
        int low = 0;
        int high = nums.length - 1;
        
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] > target) high = mid;
            else if (nums[mid] < target) low = mid + 1;
            else return mid;
        }
        return low;
    }
}
{% endhighlight %}