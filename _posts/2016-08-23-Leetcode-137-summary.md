---
layout: post
title: Leetcode Problem 137 Summary
date: 2016-08-23
categories: blog
tags: [study]

---

# 题目

Given an array of integers, every element appears three times except for one. Find that single one.

Note:  
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

# 我的算法

计算 int 中 32 位每一位出现的次数，对于只出现一次的值，其对应各位为 1 的位出现次数模 3 余 1 。

# 代码

{% highlight java %}
public class Solution {
    public int singleNumber(int[] nums) {
        int[] bits = new int[32];
        for (int i = 0; i < nums.length; i++) {
            countBits(nums[i], bits);
        }
        
        int single = 0;
        for (int i = 0; i < 32; i++) {
            single = bits[i] % 3 == 1 ? (single + (1 << i)) : single;
        }
        
        return single;
    }
    
    private void countBits(int num, int[] bits) {
        int i = 0;
        while (num != 0) {
            bits[i] += (num & 1);
            num >>>= 1;
            i++;
        }
    }
}
{% endhighlight %}

# discuss

很多很奇特的解法，[这里](https://discuss.leetcode.com/category/145/single-number-ii)