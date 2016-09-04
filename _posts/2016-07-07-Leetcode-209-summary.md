---
layout: post
title: Leetcode Problem 209 Summary
date: 2016-07-07
categories: blog
tags: [study]

---

# 题目

Given an array of n positive integers and a positive integer s, find the minimal length of a subarray of which the sum ≥ s. If there isn't one, return 0 instead.

For example, given the array [2,3,1,2,4,3] and s = 7,
the subarray [4,3] has the minimal length under the problem constraint.

More practice:  
If you have figured out the O(n) solution, try coding another solution of which the time complexity is O(n log n).

# 我的算法

考虑使用两个指针，一个start一个end，start指向当前子序列的首元素，end指向子序列的尾元素的下一个元素。

若当前子序列的和大于s则将start指针向左移一位，若小于s则将end指针向右移一位。直到end出界为止。

# 代码

{% highlight java %}
public class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        if (nums == null || nums.length == 0 || s <= 0) {
            return 0;
        }
        
        int start = 0, end = 0, minLen = Integer.MAX_VALUE, len = 0, sum = 0;
        
        while (end < (nums.length + 1)) {
            if (sum < s || start == end) {
                end++;
                len++;
                if (end > nums.length) {
                    break;
                } else {
                    sum += nums[end - 1];
                }
            } else {
                if (len < minLen) {
                    minLen = len;
                }
                sum -= nums[start];
                start++;
                len--;
            }
        }
        
        if (minLen != Integer.MAX_VALUE) {
            return minLen;
        } else {
            return 0;
        }
    }
}
{% endhighlight %}

# 二刷

{% highlight java %}
public class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int p1 = 0;
        int p2 = 0;
        int sum = 0;
        int len = Integer.MAX_VALUE;
        while (p2 < nums.length) {
            sum += nums[p2];
            if (sum >= s) {
                while (sum - nums[p1] >= s) {
                    sum -= nums[p1];
                    p1++;
                }
                len = (p2 - p1 + 1) < len ? (p2 - p1 + 1) : len; 
            }
            p2++;
        }
        return len != Integer.MAX_VALUE ? len : 0;
    }
}
{% endhighlight %}