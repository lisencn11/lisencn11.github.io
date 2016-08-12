---
layout: post
title: Leetcode Problem 266 Summary
date: 2016-08-12
categories: blog
tags: [study]

---

# 题目

Given a string, determine if a permutation of the string could form a palindrome.

For example,
"code" -> False, "aab" -> True, "carerac" -> True.

# 我的算法

刚开始想用数组计数，然后看奇偶，后来发现题目中是String，那么有可能包含各种字符，数组比较麻烦，不如直接用HashSet，如果已经存在就删去，没有就加入Set，最后看个数是否少于 2 个。

# 代码

{% highlight java %}
public class Solution {
    public boolean canPermutePalindrome(String s) {
        if (s == null || s.length() == 0) return true;
        
        Set<Character> set = new HashSet<>();
        for (int i = 0; i < s.length(); i++) {
            if (set.contains(s.charAt(i))) set.remove(s.charAt(i));
            else set.add(s.charAt(i));
        }
        
        return set.size() < 2;
    }
}
{% endhighlight %}