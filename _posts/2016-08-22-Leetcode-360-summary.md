---
layout: post
title: Leetcode Problem 360 Summary
date: 2016-08-22
categories: blog
tags: [study]

---

# 题目

Given a sorted array of integers nums and integer values a, b and c. Apply a function of the form f(x) = ax2 + bx + c to each element x in the array.

The returned array must be in sorted order.

Expected time complexity: O(n)

Example:  
nums = [-4, -2, 2, 4], a = 1, b = 3, c = 5,

Result: [3, 9, 15, 33]

nums = [-4, -2, 2, 4], a = -1, b = 3, c = 5

Result: [-23, -5, 1, 7]

# 我的算法

我分 a > 0， a = 0， a < 0 三种情况做的，看 discuss 发现 a = 0 可以合并到令两种情况。

# 代码

{% highlight java %}
public class Solution {
    public int[] sortTransformedArray(int[] nums, int a, int b, int c) {
        int[] results = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            results[i] = calculate(nums[i], a, b, c);
        }
        if (a == 0) {
            if (b < 0) {
                for (int i = 0, j = results.length - 1; i < j; i++, j--) {
                    int temp = results[i];
                    results[i] = results[j];
                    results[j] = temp;
                }
            }
            return results;
        }
        int[] res = new int[nums.length];
        if (a > 0) {
            int index = nums.length - 1;
            int p1 = 0, p2 = nums.length - 1;
            while (p1 <= p2) {
                res[index--] = results[p1] < results[p2] ? results[p2--] : results[p1++];
            }
            return res;
        }
        int index = 0;
        int p1 = 0, p2 = nums.length - 1;
        while (p1 <= p2) {
            res[index++] = results[p1] < results[p2] ? results[p1++] : results[p2--];
        }
        return res;
    }
    
    private int calculate(int num, int a, int b, int c) {
        return a * num * num + b * num + c;
    }
}
{% endhighlight %}

# discuss

{% highlight java %}
public class Solution {
    public int[] sortTransformedArray(int[] nums, int a, int b, int c) {
        int n = nums.length;
        int[] sorted = new int[n];
        int i = 0, j = n - 1;
        int index = a >= 0 ? n - 1 : 0;
        while (i <= j) {
            if (a >= 0) {
                sorted[index--] = quad(nums[i], a, b, c) >= quad(nums[j], a, b, c) ? quad(nums[i++], a, b, c) : quad(nums[j--], a, b, c);
            } else {
                sorted[index++] = quad(nums[i], a, b, c) >= quad(nums[j], a, b, c) ? quad(nums[j--], a, b, c) : quad(nums[i++], a, b, c);
            }
        }
        return sorted;
    }
    
    private int quad(int x, int a, int b, int c) {
        return a * x * x + b * x + c;
    }
}
{% endhighlight %}