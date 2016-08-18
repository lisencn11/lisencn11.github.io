---
layout: post
title: Leetcode Problem 125 Summary
date: 2016-08-18
categories: blog
tags: [study]

---

# 题目

Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

For example,  
"A man, a plan, a canal: Panama" is a palindrome.  
"race a car" is not a palindrome.

# 我的算法

two pointers 前后对比即可，条件的判断比较复杂。

discuss中用到了 Character.isLetterOrDigit() 和 Character.toLowerCase() 都对于简化条件判断很有帮助。

# 代码

{% highlight java %}
public class Solution {
    public boolean isPalindrome(String s) {
        char[] str = s.toCharArray();
        int diff = Math.abs((int)('A' - 'a'));
        
        int p1 = 0;
        int p2 = str.length - 1;
        
        while (p1 < p2) {
            while (p1 < p2 && !valid(str[p1])) p1++;
            while (p1 < p2 && !valid(str[p2])) p2--;

            if (p1 < p2) {
                int temp = Math.abs((int)(str[p1] - str[p2]));
                if (!(temp == 0 || (temp == diff && str[p1] > '9' && str[p2] > '9'))) return false;
                p1++;
                p2--;
            }
        }
        
        return true;
    }
    
    private boolean valid(char c) {
        if (c >= 'a' && c <= 'z') return true;
        if (c >= 'A' && c <= 'Z') return true;
        if (c >= '0' && c <= '9') return true;
        return false;
    }
}
{% endhighlight %}