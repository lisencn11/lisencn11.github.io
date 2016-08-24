---
layout: post
title: Leetcode Problem 340 Summary
date: 2016-08-23
categories: blog
tags: [study]

---

# 题目

Given a string, find the length of the longest substring T that contains at most k distinct characters.

For example, Given s = “eceba” and k = 2,

T is "ece" which its length is 3.

# 我的算法

用 HashMap 来判断当前重复字符数，当大于 k 时，左边指针开始向右移动，并减少 HashMap 中的计数，知道某个键值对减到 0 被删除为止，这样可以实现 O(n) 的复杂度。

# 代码

{% highlight java %}
public class Solution {
    public int lengthOfLongestSubstringKDistinct(String s, int k) {
        Map<Character, Integer> map = new HashMap<>();
        char[] str = s.toCharArray();
        int p1 = 0;
        int max = 0;
        for (int i = 0; i < str.length; i++) {
            map.put(str[i], map.containsKey(str[i]) ? map.get(str[i]) + 1 : 1);
            while (map.size() > k) {
                map.put(str[p1], map.get(str[p1]) - 1);
                if (map.get(str[p1]) == 0) map.remove(str[p1]);
                p1++;
            }
            int len = i - p1 + 1;
            max = len > max ? len : max;
        }
        return max;
    }
}
{% endhighlight %}