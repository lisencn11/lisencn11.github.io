---
layout: post
title: Leetcode Problem 260 Summary
date: 2016-08-30
categories: blog
tags: [study]

---

# 题目

Given an array of numbers nums, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.

For example:

Given nums = [1, 2, 1, 3, 2, 5], return [3, 5].

Note:  
1. The order of the result is not important. So in the above example, [5, 3] is also correct.
2. Your algorithm should run in linear runtime complexity. Could you implement it using only constant space complexity?

# 我的算法

首先对所有数异或，得到的是所求两数的异或，diff &= -diff 求得异或结果中保留最后一个 1 的值，根据这位是否为 1 将数分成两个集合，所求的两数会分别在两个集合，对这两个集合中的数做异或。

# 代码

{% highlight java %}
public class Solution {
    public int[] singleNumber(int[] nums) {
        int diff = 0;
        for (int i = 0; i < nums.length; i++) 
            diff ^= nums[i];
        
        diff &= -diff;
        
        int[] set = new int[]{0, 0};
        for (int i = 0; i < nums.length; i++) {
            if ((nums[i] & diff) == 0) {
                set[0] ^= nums[i];
            } else {
                set[1] ^= nums[i];
            }
        }
        return set;
    }
}
{% endhighlight %}