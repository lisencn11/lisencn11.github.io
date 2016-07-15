---
layout: post
title: Leetcode Problem 3 Summary
date: 2016-07-14
categories: blog
tags: [study]

---

# 题目

**输入**一个字符串。

**输出**字符串中没有重复字符的最长子串。

如：  
> Given "abcabcbb", the answer is "abc", which the length is 3.
>   
Given "bbbbb", the answer is "b", with the length of 1.
  
>Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.

# 我的算法

DP算法可以解决，使用两个pointer，分别代表子串的两端，右端pointer不断向右扫描，如果不是重复字符就加入到HashMap中，key为字符，value为index，如果遇到重复字符，则利用左右pointer计算最大长度，删除从旧左pointer到新左pointer中的HashMap，并改变左pointer。

# 代码

{% highlight java %}
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s == null || s.length() == 0) return 0;
        
        int max = 1;
        Map<Character, Integer> map = new HashMap<>();
        char[] c = s.toCharArray();
        int pre = 0;
        map.put(c[0], 0);
        for (int i = 1; i < c.length; i++) {
            if (!map.containsKey(c[i])) {
                map.put(c[i], i);
            } else {
                max = Math.max(max, i - pre);
                for (int j = pre; j < map.get(c[i]); j++)
                    map.remove(c[j]);
                pre = map.get(c[i]) + 1;
                map.put(c[i], i);
            }
        }
        max = Math.max(max, c.length - pre);
        
        return max;
    }
}
{% endhighlight %}