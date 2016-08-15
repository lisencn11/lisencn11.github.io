---
layout: post
title: Leetcode Problem 13 Summary
date: 2016-08-14
categories: blog
tags: [study]

---

# 题目

Write a function that takes an unsigned integer and returns the number of ’1' bits it has (also known as the Hamming weight).

For example, the 32-bit integer ’11' has binary representation 00000000000000000000000000001011, so the function should return 3.

# 我的算法

这里不包含算法，主要考察位操作符，需要注意的是 >> 代表有符号数的平移， >>> 代表无符号数的平移，容易出错。

# 代码

{% highlight java %}
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int ones = 0;
		while(n!=0) {
			ones = ones + (n & 1);
			n = n>>>1;
		}
		return ones;
    }
}
{% endhighlight %}