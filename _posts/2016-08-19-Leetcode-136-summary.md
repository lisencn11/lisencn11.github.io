---
layout: post
title: Leetcode Problem 136 Summary
date: 2016-08-18
categories: blog
tags: [study]

---

# 题目

Given an array of integers, every element appears twice except for one. Find that single one.

Note:  
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

# 我的算法

两个数相同的话，异或为 0 ，对数组中的每个数做异或，最后结果就是唯一那个不同的数

# 代码

{% highlight java %}
public class Solution {
    public int singleNumber(int[] nums) {
        int result = 0;
        for (int i = 0; i < nums.length; i++) {
            result ^= nums[i];
        }
        
        return result;
    }
}
{% endhighlight %}