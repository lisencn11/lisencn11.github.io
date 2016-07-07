---
layout: post
title: Leetcode Problem 209 Summary
date: 2016-07-07
categories: blog
tags: [study]

---

# 题目

**输入**一个整型数组，里面有n个整型，均为正数。一个正整数s。

**输出**找到整型数组中最小子序列，满足sum>=s，如果没有则返回0.

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