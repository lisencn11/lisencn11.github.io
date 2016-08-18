---
layout: post
title: Leetcode Problem 219 Summary
date: 2016-08-17
categories: blog
tags: [study]

---

# 题目

Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that nums[i] = nums[j] and the difference between i and j is at most k.

# 我的算法

与 217 题一样用 HashSet 解决，只是要用到滑动窗口思想，删除超过 k 长的 HashSet 中的元素。

# 代码

{% highlight java %}
public class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Set<Integer> set = new HashSet<>();
        
        int fastIter = 0;
        int slowIter = 0;
        while (fastIter < nums.length) {
            if (fastIter - slowIter > k) {
                set.remove(nums[slowIter++]);
            }
            if (!set.add(nums[fastIter++])) return true;
        }
        
        return false;
    }
}
{% endhighlight %}