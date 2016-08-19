---
layout: post
title: Leetcode Problem 168 Summary
date: 2016-08-18
categories: blog
tags: [study]

---

# 题目

Given a positive integer, return its corresponding column title as appear in an Excel sheet.

For example:

    1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB 

# 我的算法

按照26进制做，但是有几点问题：

* n 取余 26 后， Z 要单独处理，因为没有26
* 每一次都要 n = (n - 1) / 26 因为发现对于 Z = 26，不减一的话会多一次循环

# 代码

{% highlight java %}
public class Solution {
    public String convertToTitle(int n) {
        StringBuilder result = new StringBuilder();

        while (n != 0) {
            int digit = n % 26;
            digit = digit == 0 ? 26 : digit;
            n = (n - 1) / 26;
            result.insert(0, (char)(digit + (int)'A' - 1));
        }
        
        return result.toString();
    }
}
{% endhighlight %}