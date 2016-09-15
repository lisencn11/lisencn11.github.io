---
layout: post
title: Leetcode Problem 164 Summary
date: 2016-09-14
categories: blog
tags: [study]

---

# 题目

Given an unsorted array, find the maximum difference between the successive elements in its sorted form.

Try to solve it in linear time/space.

Return 0 if the array contains less than 2 elements.

You may assume all elements in the array are non-negative integers and fit in the 32-bit signed integer range.

# 我的算法

桶排序，需要注意的一点是桶大小那里要用 Math.ceil()

# 代码

{% highlight java %}
public class Solution {
    public int maximumGap(int[] nums) {
        if (nums.length < 2) return 0;
        int min = Integer.MAX_VALUE;
        int max = Integer.MIN_VALUE;
        
        for (int i = 0; i < nums.length; i++) {
            min = Math.min(min, nums[i]);
            max = Math.max(max, nums[i]);
        }
        
        if (max == min) return 0;
        
        int bucketSize = (int) Math.ceil((double) (max - min) / (double) (nums.length - 1));
        int[][] buckets = new int[nums.length][2];
        for (int[] bucket : buckets) {
            bucket[0] = -1;
            bucket[1] = -1;
        }
        
        for (int i = 0; i < nums.length; i++) {
            int bucket = (nums[i] - min) / bucketSize;
            buckets[bucket][0] = buckets[bucket][0] == -1 ? nums[i] : Math.min(buckets[bucket][0], nums[i]);
            buckets[bucket][1] = buckets[bucket][1] == -1 ? nums[i] : Math.max(buckets[bucket][1], nums[i]);
        }
        
        int pre = min;
        int result = 0;
        for (int[] bucket : buckets) {
            if (bucket[0] != -1) {
                int diff = bucket[0] - pre;
                result = Math.max(result, diff);
                pre = bucket[1];
            }
        }
        return result;
    }
}
{% endhighlight %}