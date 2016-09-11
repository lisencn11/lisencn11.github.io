---
layout: post
title: Leetcode Problem 151 Summary
date: 2016-07-22
categories: blog
tags: [study]

---

# 题目

Given an input string, reverse the string word by word.

For example,  
Given s = "the sky is blue",  
return "blue is sky the".

Update (2015-02-12):  
For C programmers: Try to solve it in-place in O(1) space.

Clarification:  

* What constitutes a word?  
A sequence of non-space characters constitutes a word.
* Could the input string contain leading or trailing spaces?  
Yes. However, your reversed string should not contain leading or trailing spaces.
* How about multiple spaces between two words?  
Reduce them to a single space in the reversed string.

# 我的算法

使用 String 的 split() 方法和 StringBuilder 类即可，注意边界情况：纯空格字符串。

# 代码

{% highlight java %}
public class Solution {
    public String reverseWords(String s) {
        if (s == null || s.length() == 0) return s;
        
        String[] str = s.split(" ");
        StringBuilder sb = new StringBuilder();
        for (int i = str.length - 1; i >= 0; i--)
            if (str[i].length() != 0) sb.append(str[i] + " ");
        if (sb.length() > 0) sb.delete(sb.length() - 1, sb.length());
        
        return sb.toString();
    }
}
{% endhighlight %}

# 二刷

{% highlight java %}
public class Solution {
    public String reverseWords(String s) {
        String[] strs = s.split(" ");
        StringBuilder sb = new StringBuilder();
        for (int i = strs.length - 1; i >= 0; i--) {
            if (!strs[i].equals("")) {
                sb.append(strs[i]);
                sb.append(" ");
            }
        }
        if (sb.length() == 0) return "";
        sb.deleteCharAt(sb.length() - 1);
        return sb.toString();
    }
}
{% endhighlight java %}