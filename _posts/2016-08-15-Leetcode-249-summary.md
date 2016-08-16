---
layout: post
title: Leetcode Problem 249 Summary
date: 2016-08-15
categories: blog
tags: [study]

---

# 题目

Given a string, we can "shift" each of its letter to its successive letter, for example: "abc" -> "bcd". We can keep "shifting" which forms the sequence:

"abc" -> "bcd" -> ... -> "xyz"  
Given a list of strings which contains only lowercase alphabets, group all strings that belong to the same shifting sequence.

For example, given: ["abc", "bcd", "acef", "xyz", "az", "ba", "a", "z"],   
A solution is:

[  
  ["abc","bcd","xyz"],  
  ["az","ba"],  
  ["acef"],  
  ["a","z"]  
]

# 我的算法

考察HashMap，主要考虑如何对字符串分组，我的思路是将当前字符串的第一个字母平移 n 位为 a ，其他各位相对应平移得到的字符串可以作为 key ，其所在的 list 的 index 可以作为 value。

# 代码

{% highlight java %}
public class Solution {
    public List<List<String>> groupStrings(String[] strings) {
        if (strings == null || strings.length == 0) return new ArrayList<>();
        
        List<List<String>> ret = new ArrayList<>();
        Map<String, Integer> map = new HashMap<>();
        int counter = 0;
        
        for (int i = 0; i < strings.length; i++) {
            String base = translate(strings[i]);
            if (!map.containsKey(base)) {
                map.put(base, counter);
                List<String> list = new ArrayList<>();
                ret.add(list);
                counter++;
            }
            ret.get(map.get(base)).add(strings[i]);
        }
        
        return ret;
    }
    
    private String translate(String string) {
        if (string == null || string.length() == 0) return string;
        
        char[] letters = string.toCharArray();
        int shift = letters[0] - 'a';
        char[] newString = new char[letters.length];
        for (int i = 0; i < letters.length; i++) {
            newString[i] = (char)(letters[i] - shift);
            if (newString[i] < 'a') newString[i] += 26;
        }
        
        return new String(newString);
    }
}
{% endhighlight %}