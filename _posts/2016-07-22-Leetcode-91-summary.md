---
layout: post
title: Leetcode Problem 91 Summary
date: 2016-07-22
categories: blog
tags: [study]

---

# 题目

A message containing letters from A-Z is being encoded to numbers using the following mapping:

'A' -> 1  
'B' -> 2  
...  
'Z' -> 26  
Given an encoded message containing digits, determine the total number of ways to decode it.

For example,  
Given encoded message "12", it could be decoded as "AB" (1 2) or "L" (12).

The number of ways decoding "12" is 2.

# 我的算法

动态规划，当前的解码组合可由之前的结果计算而得。注意处理各类输入条件，包括无法解码的条件，比如 "00" 。

# 代码

{% highlight java %}
public class Solution {
    public int numDecodings(String s) {
        if (s == null || s.length() == 0) return 0;
        if (s.charAt(0) < '1' || s.charAt(0) > '9') return 0;
        
        char[] str = s.toCharArray();
        int pre = 1;
        int prePre = 1;
        int cur = pre;
        
        for (int i = 1; i < str.length; i++) {
            if (str[i] < '0' || str[i] >'9') return 0;
            if (str[i - 1] == '0') {
                if (str[i] == '0') return 0;
                else cur = pre;
            } else if (str[i - 1] == '1') {
                if (str[i] == '0') cur = prePre;
                else cur = pre + prePre;
            } else if (str[i - 1] == '2') {
                if (str[i] == '0') cur = prePre;
                else if (str[i] >= '1' && str[i] <= '6') cur = pre + prePre;
                else cur = pre;
            } else {
                if (str[i] == '0') return 0;
                else cur = pre;
            }
            
            prePre = pre;
            pre = cur;
        }
        return cur;
    }
}
{% endhighlight %}

# 二刷

{% highlight java %}
public class Solution {
    public int numDecodings(String s) {
        if (s == null || s.length() == 0) return 0;
        if (s.charAt(0) == '0') return 0;
        int[] ways = new int[s.length()];
        ways[0] = 1;
        for (int i = 1; i < s.length(); i++) {
            if (s.charAt(i) == '0') {
                if (i - 2 >= 0 && (s.charAt(i - 1) == '1' || s.charAt(i - 1) == '2')) {
                    ways[i] = ways[i - 2];
                } else if (s.charAt(i - 1) != '1' && s.charAt(i - 1) != '2') {
                    return 0;
                } else {
                    ways[i] = 1;
                }
            } else if (s.charAt(i) >= '1' && s.charAt(i) <= '6') {
                ways[i] = ways[i - 1];
                if (i - 2 >= 0 && (s.charAt(i - 1) == '1' || s.charAt(i - 1) == '2')) {
                    ways[i] += ways[i - 2];
                } else if (i - 2 < 0 && (s.charAt(i - 1) == '1' || s.charAt(i - 1) == '2')) {
                    ways[i] += 1;
                }
            } else if (s.charAt(i) >= '7' && s.charAt(i) <= '9') {
                ways[i] = ways[i - 1];
                if (i - 2 >= 0 && (s.charAt(i - 1) == '1')) {
                    ways[i] += ways[i - 2];
                } else if (i - 2 < 0 && (s.charAt(i - 1) == '1')) {
                    ways[i] += 1;
                }
            } else {
                return 0;
            }
        }
        return ways[s.length() - 1];
    }
}
{% endhighlight %}