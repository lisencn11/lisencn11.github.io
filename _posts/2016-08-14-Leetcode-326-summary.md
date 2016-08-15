---
layout: post
title: Leetcode Problem 326 Summary
date: 2016-08-14
categories: blog
tags: [study]

---

# 题目

Given an integer, write a function to determine if it is a power of three.

# 我的算法

二分查找思路，依次找 3<sup>2</sup> 3<sup>4</sup> ... 直到大于 n 后，缩小范围为为 n / 3<sup>i</sup>

# 代码

{% highlight java %}
public class Solution {
    public boolean isPowerOfThree(int n) {
        if (n < 1) return false;
        if (n == 1) return true;
        
        long totalProduct = 1;
        while (totalProduct < n) {
            long product = 3;
            long remain = n / totalProduct;
            while (product * product < remain) {
                product *= product;
            }
            totalProduct *= product;
        }
        
        return totalProduct == n;
    }
}
{% endhighlight %}

# 无需循环的算法

1. 找到最大的 3 的 n 次方整型，看是否能整除 n
2. 看 Math.log10(n) / Math.log10(3) 结果是否为整数