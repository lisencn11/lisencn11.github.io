---
layout: post
title: Leetcode Problem 267 Summary
date: 2016-08-31
categories: blog
tags: [study]

---

# 题目

Given two strings S and T, determine if they are both one edit distance apart.

# 我的算法

one edit distance 表示 S 可以通过增删改一个字符得到 T。

# 代码

{% highlight java %}
public class Solution {
    public boolean isOneEditDistance(String s, String t) {
        Map<Character, List<Integer>> map = new HashMap<>();
        char[] strs = s.toCharArray();
        char[] strt = t.toCharArray();
        int distance = Math.abs(strs.length - strt.length);
        if (distance > 1) return false;
        boolean ret;
        if (distance == 1) ret = checkAdd(strs, strt);
        else ret = checkReplace(strs, strt);
        return ret;
    }
    
    private boolean checkAdd(char[] strs, char[] strt) {
        int p1 = 0;
        int p2 = 0;
        int count = 0;
        while (p1 < strs.length && p2 < strt.length) {
            if (strs[p1] != strt[p2]) {
                if (strs.length > strt.length) p1++;
                else p2++;
                count++;
                continue;
            }
            p1++;
            p2++;
        }
        return count == 1 || count == 0;
    }
    
    private boolean checkReplace(char[] strs, char[] strt) {
        int count = 0;
        for (int i = 0; i < strs.length; i++) {
            count = strs[i] != strt[i] ? count + 1 : count;
        }
        return count == 1;
    }
}
{% endhighlight %}

# discuss

这道题复杂度基本没法降低，但是 discuss 的代码很漂亮。

{% highlight java %}
public boolean isOneEditDistance(String s, String t) {
    for (int i = 0; i < Math.min(s.length(), t.length()); i++) { 
    	if (s.charAt(i) != t.charAt(i)) {
    		if (s.length() == t.length()) // s has the same length as t, so the only possibility is replacing one char in s and t
    			return s.substring(i + 1).equals(t.substring(i + 1));
			else if (s.length() < t.length()) // t is longer than s, so the only possibility is deleting one char from t
				return s.substring(i).equals(t.substring(i + 1));	        	
			else // s is longer than t, so the only possibility is deleting one char from s
				return t.substring(i).equals(s.substring(i + 1));
    	}
    }       
    //All previous chars are the same, the only possibility is deleting the end char in the longer one of s and t 
    return Math.abs(s.length() - t.length()) == 1;        
}
{% endhighlight %}