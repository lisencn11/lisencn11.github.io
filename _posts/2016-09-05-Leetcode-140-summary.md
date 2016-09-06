---
layout: post
title: Leetcode Problem 140 Summary
date: 2016-09-05
categories: blog
tags: [study]

---

# 题目

Given a string s and a dictionary of words dict, add spaces in s to construct a sentence where each word is a valid dictionary word.

Return all such possible sentences.

For example, given  
s = "catsanddog",  
dict = ["cat", "cats", "and", "sand", "dog"].

A solution is ["cats and dog", "cat sand dog"].
# 我的算法

用哈希表存储 String 和 List 对应关系，然后用 backtrack 做。

# 代码

{% highlight java %}
public class Solution {
    public List<String> wordBreak(String s, Set<String> wordDict) {
        Map<String, List<String>> map = new HashMap<>();
        return helper(s, wordDict, map);
    }
    
    private List<String> helper(String s, Set<String> wordDict, Map<String, List<String>> map) {
        if (map.containsKey(s)) return map.get(s);
        List<String> result = new ArrayList<>();
        for (int i = s.length() - 1; i >= 1; i--) {
            String curr = s.substring(i);
            if (wordDict.contains(curr)) {
                String next = s.substring(0, i);
                List<String> sub = helper(next, wordDict, map);
                for (String iter : sub) {
                    result.add(iter + " " + curr);
                }
            }
        }
        if (wordDict.contains(s)) result.add(s);
        map.put(s, result);
        return result;
    }
}
{% endhighlight %}