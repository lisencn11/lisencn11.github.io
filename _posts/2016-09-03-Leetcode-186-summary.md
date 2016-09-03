---
layout: post
title: Leetcode Problem 186 Summary
date: 2016-09-02
categories: blog
tags: [study]

---

# 题目

Given an input string, reverse the string word by word. A word is defined as a sequence of non-space characters.

The input string does not contain leading or trailing spaces and the words are always separated by a single space.

For example,  
Given s = "the sky is blue",  
return "blue is sky the".

Could you do it in-place without allocating extra space?

# 我的算法

1. 整体反序
2. 每个单词反序
3. 最后一个单词反序

# 代码

{% highlight java %}
public class Solution {
    public void reverseWords(char[] s) {
        int len = s.length;
        reverse(s, 0, len - 1);
        
        int start = 0;
        for (int i = 0; i < len; i++) {
            if (s[i] == ' ') {
                reverse(s, start, i - 1);
                start = i + 1;
            }
        }
        
        reverse(s, start, len - 1);
    }
    
    private void reverse(char[] s, int low, int high) {
        while (low < high) swap(s, low++, high--);
    }
    
    private void swap(char[] s, int low, int high) {
        char temp = s[low];
        s[low] = s[high];
        s[high] = temp;
    }
}
{% endhighlight %}