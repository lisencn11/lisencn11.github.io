---
layout: post
title: Leetcode Problem 238 Summary
date: 2016-08-20
categories: blog
tags: [study]

---

# 题目

Given an array of n integers where n > 1, nums, return an array output such that output[i] is equal to the product of all the elements of nums except nums[i].

Solve it without division and in O(n).

For example, given [1,2,3,4], return [24,12,8,6].

Follow up:  
Could you solve it with constant space complexity? (Note: The output array does not count as extra space for the purpose of space complexity analysis.)
# 我的算法

不允许用乘法，我的思路是：对于 output[i] ，其等于所有左边元素的乘积乘上所有右边元素的乘积，我们需要先遍历一边数组得到这两个结果在进行计算。

对于 Follow up，discuss 中给出了思路相同但极其巧妙的无需两个额外数组的方法，前缀乘积数组用output数组代替，后缀乘积数组用一个变量来存储，计算从后向前不断改变这个变量。

# 代码

{% highlight java %}
public class Solution {
    public int[] productExceptSelf(int[] nums) {
        int len = nums.length;
        int[] preProduct = new int[len];
        int[] postProduct = new int[len];
        preProduct[0] = 1;
        postProduct[len - 1] = 1;
        preProduct[1] = nums[0];
        postProduct[len - 2] = nums[len - 1];
        
        for (int i = 2, j = len - 3; i < len; i++, j--) {
            preProduct[i] = preProduct[i - 1] * nums[i - 1];
            postProduct[j] = postProduct[j + 1] * nums[j + 1];
        }
        
        int[] result = new int[len];
        for (int i = 0; i < len; i++) {
            result[i] = preProduct[i] * postProduct[i];
        }
        
        return result;
    }
}
{% endhighlight %}

# discuss

{% highlight java %}
public int[] productExceptSelf(int[] nums) {
    int n = nums.length;
    int[] res = new int[n];
    res[0] = 1;
    for (int i = 1; i < n; i++) {
        res[i] = res[i - 1] * nums[i - 1];
    }
    int right = 1;
    for (int i = n - 1; i >= 0; i--) {
        res[i] *= right;
        right *= nums[i];
    }
    return res;
}
{% endhighlight %}