---
layout: post
title: Leetcode Problem 244 Summary
date: 2016-08-24
categories: blog
tags: [study]

---

# 题目

This is a follow up of Shortest Word Distance. The only difference is now you are given the list of words and your method will be called repeatedly many times with different parameters. How would you optimize it?

Design a class which receives a list of words in the constructor, and implements a method that takes two words word1 and word2 and return the shortest distance between these two words in the list.

For example,  
Assume that words = ["practice", "makes", "perfect", "coding", "makes"].

Given word1 = “coding”, word2 = “practice”, return 3.  
Given word1 = "makes", word2 = "coding", return 1.

Note:  
You may assume that word1 does not equal to word2, and word1 and word2 are both in the list.

# 我的算法

与 243 题相比，我们需要多次查询两个 String 的最小距离，我的方法是使用 HashMap 将 String 所对应的位置存储在一个 List 中，这样避免对 String 的比较。

# 代码

{% highlight java %}
public class WordDistance {
    Map<String, List<Integer>> map;

    public WordDistance(String[] words) {
        map = new HashMap<>();
        int i = 0;
        for (String s : words) {
            if (!map.containsKey(s)) map.put(s, new ArrayList<>());
            map.get(s).add(i);
            i++;
        }
    }

    public int shortest(String word1, String word2) {
        List<Integer> list1 = map.get(word1);
        List<Integer> list2 = map.get(word2);
        
        int p1 = 0;
        int p2 = 0;
        int min = Integer.MAX_VALUE;
        while (p1 < list1.size() && p2 < list2.size()) {
            int len = Math.abs(list1.get(p1) - list2.get(p2));
            min = len < min ? len : min;
            if (list1.get(p1) < list2.get(p2)) p1++;
            else p2++;
        }
        
        return min;
    }
}

// Your WordDistance object will be instantiated and called as such:
// WordDistance wordDistance = new WordDistance(words);
// wordDistance.shortest("word1", "word2");
// wordDistance.shortest("anotherWord1", "anotherWord2");
{% endhighlight %}