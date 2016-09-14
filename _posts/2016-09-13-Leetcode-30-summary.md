---
layout: post
title: Leetcode Problem 30 Summary
date: 2016-09-13
categories: blog
tags: [study]

---

# 题目

You are given a string, s, and a list of words, words, that are all of the same length. Find all starting indices of substring(s) in s that is a concatenation of each word in words exactly once and without any intervening characters.

For example, given:
s: "barfoothefoobarman"
words: ["foo", "bar"]

You should return the indices: [0,9].
(order does not matter).

# 我的算法

我的算法是将 words 看成一个个字符，然后类似找子串，一个有效的结果起点只有可能从 index 0 到 words[0].length - 1。遍历 String 的时候用 two pointers 达到 O(n) 的复杂度，最终复杂度是 O(mn) m是 words 的长度。

然而超时了，discuss 未超时解法在[这里](https://discuss.leetcode.com/topic/35676/accepted-java-solution-12ms-with-explanation)，暂时不想和这道题纠缠了，discuss 解法代码量很大，显然不可能用于面试。

# 代码

{% highlight java %}
public class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        if (words == null || words.length == 0 || s == null || s.length() == 0) return new ArrayList<>();
        
        List<Integer> result = new ArrayList<>();
        Map<String, Integer> map = new HashMap<>();
        for (String word : words) {
            if (!map.containsKey(word)) {
                map.put(word, 1);
            }
            else map.put(word, map.get(word) + 1);
        }
        
        int wordLen = words[0].length();
        
        for (int i = 0; i < wordLen; i++) {
            int p1 = i, p2 = i;
            
            while (p2 <= s.length() - wordLen) {
                Map<String, Integer> copyMap = new HashMap<>(map);
                while (p2 <= s.length() - wordLen && copyMap.containsKey(s.substring(p2, p2 + wordLen))) {
                    
                    String key = s.substring(p2, p2 + wordLen);
                    int value = copyMap.get(key);
                    copyMap.put(key, value - 1);
                    if (value - 1 < 0) {
                        while (copyMap.get(key) < 0) {
                            String preKey = s.substring(p1, p1 + wordLen);
                            int preValue = copyMap.get(preKey);
                            copyMap.put(preKey, preValue + 1);
                            p1 += wordLen;
                        }
                    }
                    if (((p2 - p1) / wordLen) + 1 == words.length) {
                        result.add(p1);
                        String toRemove = s.substring(p1, p1 + wordLen);
                        copyMap.put(toRemove, copyMap.get(toRemove) + 1);
                        p1 += wordLen;
                    }
                    p2 += wordLen; 
                }
                p2 += wordLen;
                p1 = p2;
            }
        }
        return result;
    }
}
{% endhighlight %}