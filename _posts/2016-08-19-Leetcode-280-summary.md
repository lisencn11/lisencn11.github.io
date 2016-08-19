---
layout: post
title: Leetcode Problem 280 Summary
date: 2016-08-19
categories: blog
tags: [study]

---

# 题目

Given an unsorted array nums, reorder it in-place such that nums[0] <= nums[1] >= nums[2] <= nums[3]....

For example, given nums = [3, 5, 2, 1, 6, 4], one possible answer is [1, 6, 2, 5, 3, 4].

# 我的算法

有点像考察动态规划，在前 n - 1 的元素符合条件的情况下，第 n 个如果是奇数 index，且小于前一个元素，只需要将两者调换即可保持符合条件，不小于前一个元素不用动。

# 代码

{% highlight java %}
public class Solution {
    public void wiggleSort(int[] nums) {
        for (int i = 1; i < nums.length; i++)
            if ((i % 2 == 1) == (nums[i] < nums[i - 1])) swap(nums, i, i - 1);
    }
    
    private void swap(int[] nums, int p1, int p2) {
        int temp = nums[p1];
        nums[p1] = nums[p2];
        nums[p2] = temp;
    }
}
{% endhighlight %}