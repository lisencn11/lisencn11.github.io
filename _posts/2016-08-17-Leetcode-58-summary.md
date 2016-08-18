---
layout: post
title: Leetcode Problem 58 Summary
date: 2016-08-17
categories: blog
tags: [study]

---

# 题目

Given a string s consists of upper/lower-case alphabets and empty space characters ' ', return the length of last word in the string.

If the last word does not exist, return 0.

Note: A word is defined as a character sequence consists of non-space characters only.

For example,   
Given s = "Hello World",  
return 5.

# 我的算法

没什么好说的，数最后一个单词字母个数就好。

# 代码

{% highlight java %}
public class Solution {
    public int lengthOfLastWord(String s) {
        int iter = s.length() - 1;
        while (iter >= 0 && s.charAt(iter) == ' ') iter--;
        
        int count = 0;
        while (iter >= 0 && s.charAt(iter) != ' ') {
            iter--;
            count++;
        }
        
        return count;
    }
}
{% endhighlight %}