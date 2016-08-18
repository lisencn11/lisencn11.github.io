---
layout: post
title: Leetcode Problem 14 Summary
date: 2016-08-17
categories: blog
tags: [study]

---

# 题目

Write a function to find the longest common prefix string amongst an array of strings.

# 我的算法

没有什么算法考点。

# 代码

{% highlight java %}
public class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs == null || strs.length == 0) return new String();
        int minLen = Integer.MAX_VALUE;
        StringBuilder ret = new StringBuilder();
        
        for (int i = 0; i < strs.length; i++)
            minLen = Math.min(minLen, strs[i].length());
        
        for (int i = 0; i < minLen; i++) {
            for (int j = 0; j < strs.length - 1; j++) {
                if (strs[j].charAt(i) != strs[j + 1].charAt(i))
                    return ret.toString();
            }
            ret.append(strs[0].charAt(i));
        }
        return ret.toString();
    }
}
{% endhighlight %}