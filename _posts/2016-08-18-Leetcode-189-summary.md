---
layout: post
title: Leetcode Problem 189 Summary
date: 2016-08-18
categories: blog
tags: [study]

---

# 题目

Rotate an array of n elements to the right by k steps.

For example, with n = 7 and k = 3, the array [1,2,3,4,5,6,7] is rotated to [5,6,7,1,2,3,4].

# 我的算法

难点在于如何在 O(1) 的空间复杂度下进行 rotate 。

# 代码

O(n) 空间复杂度

{% highlight java %}
public class Solution {
    public void rotate(int[] nums, int k) {
        k = k % nums.length;
        List<Integer> list = new ArrayList<>();
        
        for (int i = nums.length - k; i < nums.length; i++)
            list.add(nums[i]);
        for (int i = 0; i < nums.length - k; i++) 
            list.add(nums[i]);
            
        for (int i = 0; i < nums.length; i++)
            nums[i] = list.get(i);
    }
}
{% endhighlight %}

O(1) 空间复杂度

{% highlight java %}
public void rotate(int[] nums, int k) {
    k %= nums.length;
    reverse(nums, 0, nums.length - 1);
    reverse(nums, 0, k - 1);
    reverse(nums, k, nums.length - 1);
}

public void reverse(int[] nums, int start, int end) {
    while (start < end) {
        int temp = nums[start];
        nums[start] = nums[end];
        nums[end] = temp;
        start++;
        end--;
    }
}
{% endhighlight %}