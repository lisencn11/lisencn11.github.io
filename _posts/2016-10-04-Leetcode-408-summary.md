---
layout: post
title: Leetcode Problem 408 Summary
date: 2016-10-04
categories: blog
tags: [study]

---

# 题目

Given a non-empty string s and an abbreviation abbr, return whether the string matches with the given abbreviation.

A string such as "word" contains only the following valid abbreviations:

["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]

Notice that only the above abbreviations are valid abbreviations of the string "word". Any other string is not a valid abbreviation of "word".

Note:  
Assume s contains only lowercase letters and abbr contains only lowercase letters and digits.

Example 1:

Given s = "internationalization", abbr = "i12iz4n":

Return true.

Example 2:

Given s = "apple", abbr = "a2e":

Return false.

# 我的算法

主要注意 corner case ，如数字以 0 开头。

# 代码

{% highlight java %}
public class Solution {
    public boolean validWordAbbreviation(String word, String abbr) {
        if (word == null && abbr == null) return true;
        if (word == null || abbr == null) return false;
        if (word.equals(abbr)) return true;
        if (word.equals("") || abbr.equals("")) return false;
        
        int wIndex = 0;
        int aIndex = 0;
        while (wIndex < word.length() && aIndex < abbr.length()) {
            char w = word.charAt(wIndex);
            char a = abbr.charAt(aIndex);
            if (a > '0' && a <= '9') {
                int end = aIndex + 1;
                while (end < abbr.length() && abbr.charAt(end) >= '0' && abbr.charAt(end) <= '9') {
                    end++;
                }
                int step = Integer.parseInt(abbr.substring(aIndex, end));
                wIndex += step;
                aIndex = end;
            } else if (a == '0') {
                return false;
            } else {
                if (a != w) return false;
                wIndex++;
                aIndex++;
            }
        }
        
        return wIndex == word.length() && aIndex == abbr.length();
    }
}
{% endhighlight %}