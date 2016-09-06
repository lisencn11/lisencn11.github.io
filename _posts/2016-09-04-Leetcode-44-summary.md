---
layout: post
title: Leetcode Problem 44 Summary
date: 2016-09-04
categories: blog
tags: [study]

---

# 题目

Implement wildcard pattern matching with support for '?' and '*'.

'?' Matches any single character.  
'*' Matches any sequence of characters (including the empty sequence).

The matching should cover the entire input string (not partial).

The function prototype should be:  
bool isMatch(const char *s, const char *p)

Some examples:  
isMatch("aa","a") → false  
isMatch("aa","aa") → true  
isMatch("aaa","aa") → false  
isMatch("aa", "*") → true  
isMatch("aa", "a*") → true  
isMatch("ab", "?*") → true  
isMatch("aab", "c*a*b") → false

# 我的算法

记录 p 的上一个星号，s 的上一个匹配终止位置。

# 代码

{% highlight java %}
public class Solution {
    public boolean isMatch(String s, String p) {
        int p1 = 0, p2 = 0, star = -1, match = 0;
        while (p1 < s.length()) {
            if (p2 < p.length() && (s.charAt(p1) == p.charAt(p2) || p.charAt(p2) == '?')) {
                p1++;
                p2++;
            } else if (p2 < p.length() && p.charAt(p2) == '*') {
                star = p2;
                match = p1;
                p2++;
            } else if (star != -1) {
                p2 = star + 1;
                match++;
                p1 = match;
            } else {
                return false;
            }
        }
        while (p2 < p.length() && p.charAt(p2) == '*') p2++;
        return p2 == p.length();
    }
}
{% endhighlight %}