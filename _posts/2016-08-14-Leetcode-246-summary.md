---
layout: post
title: Leetcode Problem 70 Summary
date: 2016-08-14
categories: blog
tags: [study]

---

# 题目

A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Write a function to determine if a number is strobogrammatic. The number is represented as a string.

For example, the numbers "69", "88", and "818" are all strobogrammatic.

# 我的算法

能够翻转后还是数字的有 0 1 6 8 9，将他们的对应关系输入 HashMap 中，从两头向中间进行比较即可。

# 代码

{% highlight java %}
public class Solution {
    public boolean isStrobogrammatic(String num) {
        Map<Character, Character> map = new HashMap<>();
        map.put('0', '0');
        map.put('1', '1');
        map.put('6', '9');
        map.put('8', '8');
        map.put('9', '6');
        
        for (int i = 0, j = num.length() - 1; i <= j; i++, j--) {
            if (!map.containsKey(num.charAt(i)) || map.get(num.charAt(i)) != num.charAt(j)) return false;
        }
        
        return true;
    }
}
{% endhighlight %}