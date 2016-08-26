---
layout: post
title: Leetcode Problem 159 Summary
date: 2016-08-25
categories: blog
tags: [study]

---

# 题目

Given a string, find the length of the longest substring T that contains at most 2 distinct characters.

For example, Given s = “eceba”,

T is "ece" which its length is 3.

# 我的算法

two pointer 计算长度

# 代码

使用 HashSet ，逻辑简单一点，效率低一点

{% highlight java %}
public class Solution {
    public int lengthOfLongestSubstringTwoDistinct(String s) {
        char[] str = s.toCharArray();
        if (str.length < 3) return str.length;
        
        Set<Character> set = new HashSet<>();
        int p1 = 0;
        int p2 = 0;
        int max = 1;
        while (p2 < str.length) {
            if (set.size() < 2) set.add(str[p2]);
            else if (!set.contains(str[p2])) {
                char remain = str[p2 - 1];
                for (int i = p2 - 2; i >=0; i--) {
                    if (str[i] != remain) {
                        set.remove(str[i]);
                        p1 = i + 1;
                        break;
                    }
                }
                set.add(str[p2]);
            }
            int len = p2 - p1 + 1;
            max = len > max ? len : max;
            p2++;
        }
        
        return max;
    }
}
{% endhighlight %}

# 更快

使用基础数据类型，并且纪录字符最近的位置使代码更快，但是代码风格很丑。

{% highlight java %}
public class Solution {
    public int lengthOfLongestSubstringTwoDistinct(String s) {
        char[] str = s.toCharArray();
        if (str.length < 3) return str.length;
        
        int p1 = 0;
        int p2 = 0;
        char c1 = ' ';
        char c2 = ' ';
        int index1 = 0;
        int index2 = 0;
        int max = 1;

        while (p2 < str.length) {
            if (c1 == ' ') {
                c1 = str[p2];
                index1 = p2;
            } else if (str[p2] != c1 && c2 == ' ') {
                c2 = str[p2];
                index2 = p2;
            } else if (str[p2] == c1) {
                index1 = p2;
            } else if (str[p2] == c2) {
                index2 = p2;
            } else if (str[p2 - 1] != c1) {
                p1 = index1 + 1;
                index1 = p2;
                c1 = str[p2];
            } else if (str[p2 - 1] != c2) {
                p1 = index2 + 1;
                index2 = p2;
                c2 = str[p2];
            }
            
            int len = p2 - p1 + 1;
            max = len > max ? len : max;
            p2++;
        }
        
        return max;
    }
}
{% endhighlight %}