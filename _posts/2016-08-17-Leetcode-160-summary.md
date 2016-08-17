---
layout: post
title: Leetcode Problem 160 Summary
date: 2016-08-17
categories: blog
tags: [study]

---

# 题目

Given a pattern and a string str, find if str follows the same pattern.

Here follow means a full match, such that there is a bijection between a letter in pattern and a non-empty word in str.

Examples:  
pattern = "abba", str = "dog cat cat dog" should return true.  
pattern = "abba", str = "dog cat cat fish" should return false.  
pattern = "aaaa", str = "dog cat cat dog" should return false.  
pattern = "abba", str = "dog dog dog dog" should return false.

Notes:  
You may assume pattern contains only lowercase letters, and str contains lowercase letters separated by a single space.

# 我的算法

对 pattern 中的字母和 str 中的字符串构成一对一的映射，检查一致性。

# 代码

{% highlight java %}
public class Solution {
    public boolean wordPattern(String pattern, String str) {
        String[] patternToStr = new String[26];
        String[] strs = str.split(" ");
        Map<String, Character> strToPattern = new HashMap<>();
        
        if (strs.length != pattern.length()) return false;
        
        for (int i = 0; i < strs.length; i++) {
            if (patternToStr[(int)(pattern.charAt(i) - 'a')] == null) 
                patternToStr[(int)(pattern.charAt(i) - 'a')] = strs[i];
            if (!strToPattern.containsKey(strs[i]))
                strToPattern.put(strs[i], pattern.charAt(i));
            if (!patternToStr[(int)(pattern.charAt(i) - 'a')].equals(strs[i]))
                return false;
            if (strToPattern.get(strs[i]) != pattern.charAt(i))
                return false;
        }
        
        return true;
    }
}
{% endhighlight %}

# discuss摘录

巧用Map.put()返回值

{% highlight java %}
public boolean wordPattern(String pattern, String str) {
    String[] words = str.split(" ");
    if (words.length != pattern.length())
        return false;
    Map index = new HashMap();
    for (Integer i=0; i<words.length; ++i)
        if (index.put(pattern.charAt(i), i) != index.put(words[i], i))
            return false;
    return true;
}
{% endhighlight %}