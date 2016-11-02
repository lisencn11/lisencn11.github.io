---
layout: post
title: Leetcode Problem 358 Summary
date: 2016-10-30
categories: blog
tags: [study]

---

# 题目

Given a non-empty string str and an integer k, rearrange the string such that the same characters are at least distance k from each other.

All input strings are given in lowercase letters. If it is not possible to rearrange the string, return an empty string "".

Example 1:

str = "aabbcc", k = 3

Result: "abcabc"

The same letters are at least distance 3 from each other.

Example 2:

str = "aaabc", k = 3 

Answer: ""

It is not possible to rearrange the string.

Example 3:

str = "aaadbbcc", k = 2

Answer: "abacabcd"

Another possible answer is: "abcabcda"

The same letters are at least distance 2 from each other.

# 我的算法

Greedy算法，维护两个数组，一个用来记录character的剩余个数，一个用来记录character下次的有效位置至少为多少。每次都选出当前index有效前提下，剩余个数最多的character。

# 代码

{% highlight java %}
public class Solution {
    
    static final int LOWERCASE_NUMBER = 26;
    
    public String rearrangeString(String str, int k) {
        int len = str.length();
        int[] count = new int[LOWERCASE_NUMBER];
        int[] valid = new int[LOWERCASE_NUMBER];
        StringBuilder result = new StringBuilder();
        
        for (int i = 0; i < len; i++) {
            count[str.charAt(i) - 'a']++;
        }
        
        for (int i = 0; i < len; i++) {
            int nextPos = findMaxNext(count, valid, i);
            if (nextPos == -1) return "";
            count[nextPos]--;
            valid[nextPos] = i + k;
            char next = (char) ('a' + nextPos);
            result.append(next);
        }
        
        return result.toString();
    }
    
    private int findMaxNext(int[] count, int[] valid, int index) {
        int result = -1;
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < LOWERCASE_NUMBER; i++) {
            if (count[i] > max && valid[i] <= index && count[i] > 0) {
                max = count[i];
                result = i;
            }
        }
        
        return result;
    }
}
{% endhighlight %}