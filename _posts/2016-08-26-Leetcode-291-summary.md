---
layout: post
title: Leetcode Problem 291 Summary
date: 2016-08-26
categories: blog
tags: [study]

---

# 题目

Given a pattern and a string str, find if str follows the same pattern.

Here follow means a full match, such that there is a bijection between a letter in pattern and a non-empty substring in str.

Examples:  
1. pattern = "abab", str = "redblueredblue" should return true.
2. pattern = "aaaa", str = "asdasdasdasd" should return true.
3. pattern = "aabb", str = "xyzabcxzyabc" should return false.

Notes:  
You may assume both pattern and str contains only lowercase letters.

# 我的算法

纠结了半天如何能高效处理，结果看了下 tags 是 backtrack，经验是以后先想最直观的解决方法，不要一下就想得到最优解。

# 代码

{% highlight java %}
public class Solution {
    Map<Character, String> map = new HashMap<>();
    Set<String> set = new HashSet<>();
    public boolean wordPatternMatch(String pattern, String str) {
        int patternLen = pattern.length(), strLen = str.length();
        if (strLen == 0 && patternLen == 0) return true;
        if (strLen == 0 || patternLen == 0) return false;

        String subPattern = pattern.substring(0, patternLen - 1);
        char key = pattern.charAt(patternLen - 1);
        
        if (map.containsKey(key)) {
            String value = map.get(key);
            int valueLen = value.length();
            if (strLen - valueLen >= 0 && value.equals(str.substring(strLen - valueLen, strLen))) {
                if (wordPatternMatch(subPattern, str.substring(0, strLen - valueLen))) return true;
                else return false;
            } else {
                return false;
            }
        }
        
        for (int i = strLen - 1; i >= patternLen - 1; i--) {
            String subStr = str.substring(0, i);
            String value = str.substring(i, strLen);
            if (!set.add(value)) continue;
            map.put(key, value);
            if (wordPatternMatch(subPattern, subStr)) return true;
            map.remove(key);
            set.remove(value);
        }
        return false;
    }
}
{% endhighlight %}