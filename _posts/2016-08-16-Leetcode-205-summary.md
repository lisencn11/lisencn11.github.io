---
layout: post
title: Leetcode Problem 205 Summary
date: 2016-08-16
categories: blog
tags: [study]

---

# 题目

Given two strings s and t, determine if they are isomorphic.

Two strings are isomorphic if the characters in s can be replaced to get t.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character but a character may map to itself.

For example,  
Given "egg", "add", return true.

Given "foo", "bar", return false.

Given "paper", "title", return true.

Note:  
You may assume both s and t have the same length.

# 我的算法

字母字母是一一映射关系，先用HashMap搞定从s到t的映射，在用HashSet确定t到s是一对一还是一堆多。

# 代码

{% highlight java %}
public class Solution {
    public boolean isIsomorphic(String s, String t) {
        if (s == null && t == null) return true;
        if (s == null || t == null) return false;
        if (s.length() != t.length()) return false;
        
        Map<Character, Character> map = new HashMap<>();
        for (int i = 0; i < s.length(); i++) {
            if (!map.containsKey(t.charAt(i))) map.put(t.charAt(i), s.charAt(i));
            if (map.get(t.charAt(i)) != s.charAt(i)) return false;
        }
        
        Set<Character> set = new HashSet<>();
        for (Map.Entry entry : map.entrySet())
            if (!set.add((char)entry.getValue())) return false;
        
        return true;
    }
}
{% endhighlight %}