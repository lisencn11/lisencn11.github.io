---
layout: post
title: Leetcode Problem 416 Summary
date: 2016-10-15
categories: blog
tags: [study]

---

# 题目

Given a non-empty array containing only positive integers, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

Note:

1. Each of the array element will not exceed 100.
2. The array size will not exceed 200.

Example 1:

Input: [1, 5, 11, 5]

Output: true

Explanation: The array can be partitioned as [1, 5, 5] and [11].

Example 2:

Input: [1, 2, 3, 5]

Output: false

Explanation: The array cannot be partitioned into equal sum subsets.

# 我的算法

先算出总和sum，等同于问数组中是否存在一个集合加和为sum/2。

DP算法：sumExist[i]表示能够找到一个集合加和为i。

# 代码

{% highlight java %}
public class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
        }
        
        if (sum % 2 == 1) return false;
        int half = sum / 2;
        
        boolean[] sumExist = new boolean[half + 1];
        sumExist[0] = true;
        
        for (int i = 0; i < nums.length; i++) {
            for (int j = half; j >= nums[i]; j--) {
                sumExist[j] = sumExist[j] || sumExist[j - nums[i]];
            }
        }
        
        return sumExist[half];
    }
}
{% endhighlight %}