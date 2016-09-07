---
layout: post
title: Leetcode Problem 336 Summary
date: 2016-09-06
categories: blog
tags: [study]

---

# 题目

Given a list of unique words. Find all pairs of distinct indices (i, j) in the given list, so that the concatenation of the two words, i.e. words[i] + words[j] is a palindrome.

Example 1:  
Given words = ["bat", "tab", "cat"]  
Return [[0, 1], [1, 0]]  
The palindromes are ["battab", "tabbat"]  
Example 2:  
Given words = ["abcd", "dcba", "lls", "s", "sssll"]  
Return [[0, 1], [1, 0], [3, 2], [2, 4]]  
The palindromes are ["dcbaabcd", "abcddcba", "slls", "llssssll"]

# 我的算法

Hash Table 存储每个字符串，对每个字符串的各个前缀子串和后缀子串进行判断是否回文，然后是否存在和另一子串相反的字符串。

# 代码

{% highlight java %}
public class Solution {
    public List<List<Integer>> palindromePairs(String[] words) {
        Map<String, Integer> map = new HashMap<>();
        List<List<Integer>> ret = new ArrayList<>();
        for (int i = 0; i < words.length; i++) map.put(words[i], i);
        
        for (int i = 0; i < words.length; i++) {
            for (int j = 0; j <= words[i].length(); j++) {
                String str1 = words[i].substring(0, j);
                String str2 = words[i].substring(j);
                if (isPalindrome(str1)) {
                    String str2rvs = new StringBuilder(str2).reverse().toString();
                    if (map.containsKey(str2rvs) && map.get(str2rvs) != i) {
                        List<Integer> list = new ArrayList<>();
                        list.add(map.get(str2rvs));
                        list.add(i);
                        ret.add(list);
                    }
                }
                if (isPalindrome(str2)) {
                    String str1rvs = new StringBuilder(str1).reverse().toString();
                    if (map.containsKey(str1rvs) && map.get(str1rvs) != i && str2.length() != 0) {
                        List<Integer> list = new ArrayList<>();
                        list.add(i);
                        list.add(map.get(str1rvs));
                        ret.add(list);
                    }
                }
            }
        }
        return ret;
    }
    
    private boolean isPalindrome(String s) {
        int p1 = 0, p2 = s.length() - 1;
        while (p1 < p2) {
            if (s.charAt(p1++) != s.charAt(p2--)) return false;
        }
        return true;
    }
}
{% endhighlight %}