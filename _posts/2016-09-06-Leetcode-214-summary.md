---
layout: post
title: Leetcode Problem 214 Summary
date: 2016-09-06
categories: blog
tags: [study]

---

# 题目

Given a string S, you are allowed to convert it to a palindrome by adding characters in front of it. Find and return the shortest palindrome you can find by performing this transformation.

For example:

Given "aacecaaa", return "aaacecaaa".

Given "abcd", return "dcbabcd".

# 我的算法

我用的很直接的方法，discuss 中有用 KMP 做的，实在不认为面试现场能让我写 KMP ，先挖个坑，等刷完一遍再来填吧。。。

# 代码

{% highlight java %}
public class Solution {
    public String shortestPalindrome(String s) {
        char[] str = s.toCharArray();
        int len = str.length;
        int p1 = 0, p2 = len - 1;
        while (p1 < p2) {
            if (str[p1] == str[p2]) {
                boolean palindrome = true;
                for (int i = p1, j = p2; i < j; i++, j--) {
                    if (str[i] != str[j]) {
                        palindrome = false;
                        break;
                    }
                }
                if (palindrome) {
                    break;
                } else {
                    p2--;
                }
            } else {
                p2--;
            }
        }
        StringBuilder sb = new StringBuilder();
        for (int i = len - 1; i > p2; i--) {
            sb.append(str[i]);
        }
        sb.append(s);
        return sb.toString();
    }
}
{% endhighlight %}