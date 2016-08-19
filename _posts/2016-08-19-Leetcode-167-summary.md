---
layout: post
title: Leetcode Problem 167 Summary
date: 2016-08-18
categories: blog
tags: [study]

---

# 题目

Given an array of integers that is already sorted in ascending order, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are not zero-based.

You may assume that each input would have exactly one solution.

Input: numbers={2, 7, 11, 15}, target=9  
Output: index1=1, index2=2

# 我的算法

因为数组已经有序，可以使用两个指针，一个指向开头一个指向尾端，若加和大于target，尾端指针减一，反之开头指针加一，直到加和相等。

# 代码

{% highlight java %}
public class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int p1 = 0;
        int p2 = numbers.length - 1;
        
        while (p1 < p2) {
            if (numbers[p1] + numbers[p2] < target) p1++;
            else if (numbers[p1] + numbers[p2] > target) p2--;
            else break;
        }
        
        int[] ret = new int[2];
        ret[0] = p1 + 1;
        ret[1] = p2 + 1;
        return ret;
    }
}
{% endhighlight %}