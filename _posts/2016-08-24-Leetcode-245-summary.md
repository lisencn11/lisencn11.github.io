---
layout: post
title: Leetcode Problem 245 Summary
date: 2016-08-24
categories: blog
tags: [study]

---

# 题目

This is a follow up of Shortest Word Distance. The only difference is now word1 could be the same as word2.

Given a list of words and two words word1 and word2, return the shortest distance between these two words in the list.

word1 and word2 may be the same and they represent two individual words in the list.

For example,  
Assume that words = ["practice", "makes", "perfect", "coding", "makes"].

Given word1 = “makes”, word2 = “coding”, return 1.  
Given word1 = "makes", word2 = "makes", return 3.

Note:  
You may assume word1 and word2 are both in the list.

# 我的算法

与 243 题类似，只是允许相同 word1 和 word2 是相同元素了。我的算法是每次判断完 word1 就进行距离更新，这样既是 word2 相同，仍然能计算到上次的距离，然后排除 0 距离即可。

# 代码

{% highlight java %}
public class Solution {
    public int shortestWordDistance(String[] words, String word1, String word2) {
        int p1 = -1;
        int p2 = -1;
        int min = Integer.MAX_VALUE;
        for (int i = 0; i < words.length; i++) {
            if (words[i].equals(word1)) {
                p1 = i;
                min = (p2 != -1 && p1 - p2 < min && p1 - p2 != 0) ? p1 - p2 : min;
            }
            if (words[i].equals(word2)) {
                p2 = i;
                min = (p1 != -1 && p2 - p1 < min && p2 - p1 != 0) ? p2 - p1 : min;
            }
        }
        return min;
    }
}
{% endhighlight %}