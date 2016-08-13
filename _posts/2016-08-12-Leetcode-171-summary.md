---
layout: post
title: Leetcode Problem 346 Summary
date: 2016-08-12
categories: blog
tags: [study]

---

# 题目

Given a column title as appear in an Excel sheet, return its corresponding column number.

For example:

    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28 

# 我的算法

简单来看此题就是在考察26进制而已。

# 代码

{% highlight java %}
public class Solution {
    public int titleToNumber(String s) {
        int sum = 0;
        for (int i = 0; i < s.length(); i++) {
            int digit = s.charAt(i) - 'A' + 1;
            sum = sum * 26 + digit;
        }
        return sum;
    }
}
{% endhighlight %}