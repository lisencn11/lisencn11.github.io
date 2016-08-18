---
layout: post
title: Leetcode Problem 28 Summary
date: 2016-08-18
categories: blog
tags: [study]

---

# 题目

Implement strStr().

Returns the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

# 我的算法

字符串匹配问题， easy 级别不要求 KMP ，但下面贴了我的 KMP 代码复习一下 KMP 。

# 代码

{% highlight java %}
public class Solution {
    public int strStr(String haystack, String needle) {
        if (needle == null || needle.length() == 0) return 0;
        for (int i = 0; i <= (haystack.length() - needle.length()); i++) {
            int offset = 0;
            for (int j = 0; j < needle.length(); j++) {
                if (haystack.charAt(i + offset) != needle.charAt(j)) break;
                else offset++;
            }
            if (offset == needle.length()) return i;
        }
        
        return -1;
    }
}
{% endhighlight %}

# KMP

{% highlight java %}

{% endhighlight %}