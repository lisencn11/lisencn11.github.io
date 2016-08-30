---
layout: post
title: Leetcode Problem 80 Summary
date: 2016-08-28
categories: blog
tags: [study]

---

# 题目

Follow up for "Remove Duplicates":
What if duplicates are allowed at most twice?

For example,  
Given sorted array nums = [1,1,1,2,2,3],

Your function should return length = 5, with the first five elements of nums being 1, 1, 2, 2 and 3. It doesn't matter what you leave beyond the new length.

# 我的算法

two pointers

# 代码

{% highlight java %}
public class Solution {
    public int removeDuplicates(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        int p1 = 0;
        int p2 = 0;

        while (p2 < nums.length) {
            if (p1 == 0 || p1 == 1 || (nums[p2] == nums[p1 - 1] && nums[p1 - 2] != nums[p1 - 1]) || nums[p2] > nums[p1 - 1]) nums[p1++] = nums[p2];
            
            p2++;
        }
        
        return p1;
    }
}
{% endhighlight %}