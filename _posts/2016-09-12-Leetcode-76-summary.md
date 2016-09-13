---
layout: post
title: Leetcode Problem 76 Summary
date: 2016-09-12
categories: blog
tags: [study]

---

# 题目

Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).

For example,
S = "ADOBECODEBANC"
T = "ABC"
Minimum window is "BANC".

Note:
If there is no such window in S that covers all characters in T, return the empty string "".

If there are multiple such windows, you are guaranteed that there will always be only one unique minimum window in S.

# 我的算法

p2 p1 两个指针表示一段 String，用 HashMap 记录 T 字符串各字符出现的次数，HashSet 记录当前 p1 p2 字符串中所缺的字符，p2 右移时遇到 HashMap 中存储的元素就把它的次数减一，到 0 则从HashSet 中删除这个元素；p1 右移时相反遇到元素次数加一，大于 0 则向 HashSet 中添加元素。当 set 为空表示当前是合法字符串，执行 p1 右移，当 set 不为空表示当前不符合条件，需要 p2 右移扩大字符串。

初始化 min 为 s + " " 若返回时 min 的长度比 s 字符串还长则表示没找到符合条件的字符串。

# 代码

{% highlight java %}
public class Solution {
    public String minWindow(String s, String t) {
        Set<Character> set = new HashSet<>();
        Map<Character, Integer> map = new HashMap<>();
        
        for (int i = 0; i < t.length(); i++) {
            set.add(t.charAt(i));
            if (!map.containsKey(t.charAt(i))) map.put(t.charAt(i), 1);
            else map.put(t.charAt(i), map.get(t.charAt(i)) + 1);
        }
        
        int p1 = 0;
        int p2 = 0;
        String min = s + " ";
        while (p2 < s.length()) {
            while (!set.isEmpty() && p2 < s.length()) {
                char c = s.charAt(p2);
                if (map.containsKey(c)) {
                    int value = map.get(c);
                    map.put(c, value - 1);
                    if (value - 1 == 0) set.remove(c);
                }
                p2++;
            }
            while (set.isEmpty() && p1 < p2) {
                char c = s.charAt(p1);
                min = min.length() > p2 - p1 ? s.substring(p1, p2) : min;
                if (map.containsKey(c)) {
                    int value = map.get(c);
                    map.put(c, value + 1);
                    if (value + 1 > 0) set.add(c);
                }
                p1++;
            }
        }
        return min.length() > s.length() ? "" : min;
    }
}
{% endhighlight %}