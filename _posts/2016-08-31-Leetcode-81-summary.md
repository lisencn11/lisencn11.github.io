---
layout: post
title: Leetcode Problem 275 Summary
date: 2016-08-31
categories: blog
tags: [study]

---

# 题目

Follow up for "Search in Rotated Sorted Array":
What if duplicates are allowed?

Would this affect the run-time complexity? How and why?

Write a function to determine if a given target is in the array.

# 我的算法

二分查找，如果发现起点和下一个点的值相同就 ++ ， high 同理 -- 保证能够确定 nums[mid] 是在哪一段。

# 代码

{% highlight java %}
public class Solution {
    public boolean search(int[] nums, int target) {
        int low = 0;
        int high = nums.length - 1;
        
        while (low < high) {
            if (nums[low] == nums[low + 1]) {
                low++;
                continue;
            }
            if (nums[high] == nums[high - 1]) {
                high--;
                continue;
            }
            int mid = low + (high - low) / 2;
            if (nums[mid] == target) return true;
            if (nums[mid] >= nums[low]) {
                if (target < nums[mid] && target >= nums[low]) high = mid - 1;
                else low = mid + 1;
            } else {
                if (target > nums[mid] && target <= nums[high]) low = mid + 1;
                else high = mid - 1;
            }
        }
        
        return nums[low] == target;
    }
}
{% endhighlight %}