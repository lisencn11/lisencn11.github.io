---
layout: post
title: Leetcode Problem 153 Summary
date: 2016-08-24
categories: blog
tags: [study]

---

# 题目

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

Find the minimum element.

You may assume no duplicate exists in the array.

# 我的算法

因为数组中不能有重复元素，所以简单很多，二分查找，主要任务判断 mid 元素落在大于 nums[0] 的那边还是小于 nums[nums.length - 1] 那边。

# 代码

{% highlight java %}
public class Solution {
    public int findMin(int[] nums) {
        if (nums.length == 1 || nums[0] < nums[nums.length - 1]) return nums[0];
        int low = 0;
        int high = nums.length - 1;
        
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] > nums[nums.length - 1]) low = mid + 1;
            if (nums[mid] < nums[0]) high = mid;
        }
        
        return nums[low];
    }
}
{% endhighlight %}