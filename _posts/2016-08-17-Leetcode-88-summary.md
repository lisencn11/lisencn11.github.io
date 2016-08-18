---
layout: post
title: Leetcode Problem 88 Summary
date: 2016-08-17
categories: blog
tags: [study]

---

# 题目

Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.

Note:  
You may assume that nums1 has enough space (size that is greater or equal to m + n) to hold additional elements from nums2. The number of elements initialized in nums1 and nums2 are m and n respectively.

# 我的算法

从前向后存在 num1 数组元素被覆盖的问题，从后向前 merge 就没有问题了。

# 代码

{% highlight java %}
public class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int numsIter1 = m - 1;
        int numsIter2 = n - 1;
        int iter = m + n - 1;
        
        while (numsIter1 >= 0 && numsIter2 >= 0) {
            if (nums1[numsIter1] > nums2[numsIter2]) nums1[iter--] = nums1[numsIter1--];
            else nums1[iter--] = nums2[numsIter2--];
        }
        
        while (numsIter2 >= 0) nums1[iter--] = nums2[numsIter2--];
    }
}
{% endhighlight %}