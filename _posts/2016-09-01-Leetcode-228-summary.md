---
layout: post
title: Leetcode Problem 228 Summary
date: 2016-09-01
categories: blog
tags: [study]

---

# 题目

Given a sorted integer array without duplicates, return the summary of its ranges.

For example, given [0,1,2,4,5,7], return ["0->2","4->5","7"].

# 我的算法

数组操作

# 代码

{% highlight java %}
public class Solution {
    public List<String> summaryRanges(int[] nums) {
        int start = 0, end = 0;
        List<String> ret = new ArrayList<>();
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] == nums[i - 1] + 1) {
                end = i;
            } else {
                if (start == end) {
                    ret.add(Integer.toString(nums[start]));
                } else {
                    ret.add(Integer.toString(nums[start]) + "->" + Integer.toString(nums[end]));
                }
                start = i;
                end = i;
            }
        }
        if (start == nums.length - 1) {
            ret.add(Integer.toString(nums[start]));
        } else if (start != end) {
            ret.add(Integer.toString(nums[start]) + "->" + Integer.toString(nums[end]));
        }
        return ret;
    }
}
{% endhighlight %}