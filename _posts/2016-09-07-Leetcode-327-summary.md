---
layout: post
title: Leetcode Problem 327 Summary
date: 2016-09-07
categories: blog
tags: [study]

---

# 题目

Given an integer array nums, return the number of range sums that lie in [lower, upper] inclusive.
Range sum S(i, j) is defined as the sum of the elements in nums between indices i and j (i ≤ j), inclusive.

Note:  
A naive algorithm of O(n2) is trivial. You MUST do better than that.

Example:  
Given nums = [-2, 5, -1], lower = -2, upper = 2,  
Return 3.  
The three ranges are : [0, 0], [2, 2], [0, 2] and their respective sums are: -2, -1, 2.

# 我的算法

分治算法。

# 代码

{% highlight java %}
public class Solution {
    public int countRangeSum(int[] nums, int lower, int upper) {
        int n = nums.length;
        long[] sum = new long[n + 1];
        for (int i = 0; i < n; i++) {
            sum[i + 1] = sum[i] + nums[i];
        }
        return merge(sum, 0, n + 1, lower, upper);
    }
    
    private int merge(long[] sum, int start, int end, int lower, int upper) {
        if (end - start <= 1) return 0;
        int mid = start + (end - start) / 2;
        int count = merge(sum, start, mid, lower, upper)
                  + merge(sum, mid, end, lower, upper);
        long[] cache = new long[end - start];
        int lowBound = mid, highBound = mid, i2 = mid;
        for (int i = start, iter = 0; i < mid; i++, iter++) {
            while (lowBound < end && sum[lowBound] - sum[i] < lower) lowBound++;
            while (highBound < end && sum[highBound] - sum[i] <= upper) highBound++;
            while (i2 < end && sum[i2] < sum[i]) cache[iter++] = sum[i2++];
            cache[iter] = sum[i];
            count += highBound - lowBound;
        }
        System.arraycopy(cache, 0, sum, start, i2 - start);
        return count;
    }
}
{% endhighlight %}