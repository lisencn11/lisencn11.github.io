---
layout: post
title: Leetcode Problem 231 Summary
date: 2016-08-14
categories: blog
tags: [study]

---

# 题目

Write a program to check whether a given number is an ugly number.

Ugly numbers are positive numbers whose prime factors only include 2, 3, 5. For example, 6, 8 are ugly while 14 is not ugly since it includes another prime factor 7.

Note that 1 is typically treated as an ugly number.

# 我的算法

最直接的尝试除到1为止，并没看到有更加 tricky 的方法。

# 代码

{% highlight java %}
public class Solution {
    public boolean isUgly(int num) {
        if (num == 0) return false;
        if (num == 1 || num == 2 || num == 3 || num == 5) return true;
        else return (num % 2 == 0 && isUgly(num / 2)) || (num % 3 == 0 && isUgly(num / 3)) || (num % 5 == 0 && isUgly(num / 5));
    }
}
{% endhighlight %}