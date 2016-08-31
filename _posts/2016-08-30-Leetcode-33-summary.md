---
layout: post
title: Leetcode Problem 33 Summary
date: 2016-08-30
categories: blog
tags: [study]

---

# 题目

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

# 我的算法

这道题刚开始没想明白，直接先用 binary search 求了最低点，其实没必要，discuss的解法是不需要求最低点的。

# 代码

{% highlight java %}
public class Solution {
    public int search(int[] nums, int target) {
        if (nums == null || nums.length == 0) return -1;
        
        if (nums[0] <= nums[nums.length - 1]) {
            int ret = Arrays.binarySearch(nums, target);
            if (ret >= 0) return ret;
            else return -1;
        }
        
        
        int lowest = searchLowest(nums);
        int low = -1;
        int high = -1;
        if (target >= nums[0]) {
            low = 0;
            high = lowest - 1;
        } else {
            low = lowest;
            high = nums.length - 1;
        }
        
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (target < nums[mid]) {
                high = mid - 1;
            } else if (target > nums[mid]) {
                low = mid + 1;
            } else {
                return mid;
            }
        }
        return -1;
    }
    
    private int searchLowest(int[] nums) {
        int low = 0;
        int high = nums.length - 1;
        
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] >= nums[0]) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return low;
    }
}
{% endhighlight %}

# discuss

{% highlight java %}
public class Solution {
    public int search(int[] nums, int target) {
        int start = 0;
        int end = nums.length - 1;
        while (start <= end){
            int mid = (start + end) / 2;
            if (nums[mid] == target)
                return mid;
        
            if (nums[start] <= nums[mid]){
                 if (target < nums[mid] && target >= nums[start]) 
                    end = mid - 1;
                 else
                    start = mid + 1;
            } 
        
            if (nums[mid] <= nums[end]){
                if (target > nums[mid] && target <= nums[end])
                    start = mid + 1;
                 else
                    end = mid - 1;
            }
        }
        return -1;
    }
}
{% endhighlight %}