---
layout: post
title: Leetcode Problem 242 Summary
date: 2016-08-12
categories: blog
tags: [study]

---

# 题目

Given two strings s and t, write a function to determine if t is an anagram of s.

For example,  
s = "anagram", t = "nagaram", return true.  
s = "rat", t = "car", return false.

Note:  
You may assume the string contains only lowercase alphabets.

# 我的算法

算法就是对每个字符出现的次数进行计数，对应字符出现次数相同的话则是valid anagram。没有注意题中的只有小写字母，否则可以直接用计数排序。

# 代码

{% highlight java %}
public class Solution {
    public boolean isAnagram(String s, String t) {
        Map<Character, Integer> map = new HashMap<>();
        
        for (int i = 0; i < s.length(); i++) {
            if (!map.containsKey(s.charAt(i))) map.put(s.charAt(i), 0);
            map.put(s.charAt(i), map.get(s.charAt(i)) + 1);
        }
        
        for (int i = 0; i < t.length(); i++) {
            if (!map.containsKey(t.charAt(i)) || map.get(t.charAt(i)) == 0) return false;
            map.put(t.charAt(i), map.get(t.charAt(i)) - 1);
        }
        
        for (Map.Entry entry : map.entrySet()) {
            if ((int)entry.getValue() != 0) return false;
        }
        
        return true;
    }
}
{% endhighlight %}