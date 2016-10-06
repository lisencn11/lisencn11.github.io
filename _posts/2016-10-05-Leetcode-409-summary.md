---
layout: post
title: Leetcode Problem 409 Summary
date: 2016-10-05
categories: blog
tags: [study]

---

# 题目

Given a string which consists of lowercase or uppercase letters, find the length of the longest palindromes that can be built with those letters.

This is case sensitive, for example "Aa" is not considered a palindrome here.

Note:  
Assume the length of given string will not exceed 1,010.

Example:

Input:  
"abccccdd"

Output:  
7

Explanation:  
One longest palindrome that can be built is "dccaccd", whose length is 7.

# 我的算法

对每个字母进行计数，奇数偶数分别处理即可。

# 代码

{% highlight java %}
public class Solution {
    public int longestPalindrome(String s) {
        int[] count = new int[52];
        int length = 0;
        boolean odd = false;
        
        for (int i = 0; i < s.length(); i++) {
            count[getIndex(s.charAt(i))]++;
        }
        for (int i = 0; i < count.length; i++) {
            length += (count[i] % 2) == 1 ? count[i] - 1 : count[i];
            odd = (count[i] % 2) == 1 ? true : odd;
        }
        length = odd ? length + 1 : length;
        return length;
    }
    
    private int getIndex(char c) {
        if (c >= 'a' && c <= 'z') {
            return (int) (c - 'a');
        } else if (c >= 'A' && c <= 'Z') {
            return (int) (c - 'A') + 26;
        } else return -1;
    }
}
{% endhighlight %}