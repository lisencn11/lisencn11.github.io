---
layout: post
title: Leetcode Problem 65 Summary
date: 2016-09-11
categories: blog
tags: [study]

---

# 题目

Given an unsorted integer array, find the first missing positive integer.

For example,  
Given [1,2,0] return 3,  
and [3,4,-1,1] return 2.

Your algorithm should run in O(n) time and uses constant space.

# 我的算法

将范围内的数 num[i] 放入 num[num[i]] 中，思想与桶排序类似，最后看第一个空的桶的位置就是缺失的正整型。

# 代码

{% highlight java %}
public class Solution {
    public int firstMissingPositive(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            int index = i;
            while (nums[index] >= 1 && nums[index] <= nums.length && nums[index] != nums[nums[index] - 1]) {
                int temp = nums[nums[index] - 1];
                nums[nums[index] - 1] = nums[index];
                nums[index] = temp;
            }
        }
        
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != i + 1) return i + 1;
        }
        return nums.length + 1;
    }
}
{% endhighlight %}