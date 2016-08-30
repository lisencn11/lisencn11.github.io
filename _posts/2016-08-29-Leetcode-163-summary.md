---
layout: post
title: Leetcode Problem 163 Summary
date: 2016-08-29
categories: blog
tags: [study]

---

# 题目

Given a sorted integer array where the range of elements are [lower, upper] inclusive, return its missing ranges.

For example, given [0, 1, 3, 50, 75], lower = 0 and upper = 99, return ["2", "4->49", "51->74", "76->99"].

# 我的算法

好像并不需要什么算法。

# 代码

{% highlight java %}
public class Solution {
    public List<String> findMissingRanges(int[] nums, int lower, int upper) {
        List<String> list = new ArrayList<>();
        
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == lower) lower++;
            else {
                int len = nums[i] - lower;
                String s;
                if (len == 1) s = Integer.toString(lower);
                else s = Integer.toString(lower) + "->" + Integer.toString(nums[i] - 1);
                list.add(s);
                lower = nums[i] + 1;
            }
        }
        if (lower == upper) list.add(Integer.toString(lower));
        if (lower < upper) list.add(Integer.toString(lower) + "->" + Integer.toString(upper));
        return list;
    }
}
{% endhighlight %}