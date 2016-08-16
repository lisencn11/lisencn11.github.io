---
layout: post
title: Leetcode Problem 26 Summary
date: 2016-08-15
categories: blog
tags: [study]

---

# 题目

Given a sorted array, remove the duplicates in place such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this in place with constant memory.

For example,  
Given input array nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively. It doesn't matter what you leave beyond the new length.

# 我的算法

two pointers 一个指向需要替换的元素，一个指向下一个非重复元素。

# 代码

{% highlight java %}
public class Solution {
    public int removeDuplicates(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        
        int p1 = 1, p2 = 1;
        while (p2 < nums.length) {
            if (p1 < nums.length && nums[p1] > nums[p1 - 1]) {
                p1++;
                p2++;
                continue;
            }
            
            while (p2 < nums.length && nums[p2] == nums[p2 - 1]) p2++;
            if (p2 == nums.length) break;
            nums[p1++] = nums[p2++];
        }
        
        return p1;
    }
}
{% endhighlight %}

### 更简洁

{% highlight java %}
public class Solution {
    public int removeDuplicates(int[] nums) {
        int i = nums.length > 0 ? 1 : 0;
        for (int n : nums)
            if (n > nums[i-1])
                nums[i++] = n;
        return i;
    }
}
{% endhighlight %}