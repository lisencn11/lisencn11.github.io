---
layout: post
title: Leetcode Problem 344 Summary
date: 2016-08-12
categories: blog
tags: [study]

---

# 题目

字符串逆序

# 我的算法

O(n)算法，用一个StringBuilder即可

# 代码

{% highlight java %}
public class Solution {
    public String reverseString(String s) {
        StringBuilder sb = new StringBuilder();
        for (int i = s.length() - 1; i >= 0; i--) {
            sb.append(s.charAt(i));
        }
        return sb.toString();
    }
}
{% endhighlight %}