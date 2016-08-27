---
layout: post
title: Leetcode Problem 75 Summary
date: 2016-08-26
categories: blog
tags: [study]

---

# 题目

Given an array with n objects colored red, white or blue, sort them so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

Note:  
You are not suppose to use the library's sort function for this problem.

# 我的算法

我用的桶排序，discuss 用的 two pointers

# 代码

{% highlight java %}
public class Solution {
    public void sortColors(int[] nums) {
        int zero = 0;
        int one = 0;
        int two = 0;
        
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 0) zero++;
            else if (nums[i] == 1) one++;
            else two++;
        }
        
        int p = 0;
        while (p < nums.length) {
            if (zero != 0) {
                nums[p++] = 0;
                zero--;
            } else if (one != 0) {
                nums[p++] = 1;
                one--;
            } else nums[p++] = 2;
        }
    }
}
{% endhighlight %}

# discuss

{% highlight cpp %}
void sortColors(int A[], int n) {
    int j = 0, k = n-1;
    for (int i=0; i <= k; i++) {
        if (A[i] == 0)
            swap(A[i], A[j++]);
        else if (A[i] == 2)
            swap(A[i--], A[k--]);
    }
}
{% endhighlight %}