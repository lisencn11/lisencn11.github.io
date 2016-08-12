---
layout: post
title: Leetcode Problem 243 Summary
date: 2016-08-12
categories: blog
tags: [study]

---

# 题目

Given a list of words and two words word1 and word2, return the shortest distance between these two words in the list.

For example,
Assume that words = ["practice", "makes", "perfect", "coding", "makes"].

Given word1 = “coding”, word2 = “practice”, return 3.  
Given word1 = "makes", word2 = "coding", return 1.

Note:
You may assume that word1 does not equal to word2, and word1 and word2 are both in the list.

# 我的算法

遍历并记录两个String的位置即可。

# 代码

{% highlight java %}
public class Solution {
    public int shortestDistance(String[] words, String word1, String word2) {
        int pos1 = -1;
        int pos2 = -1;
        int len = Integer.MAX_VALUE;
        int iter = 0;
        boolean changed = false;
        
        for (String word : words) {
            if (word.equals(word1)) {
                pos1 = iter;
                changed = true;
            } else if (word.equals(word2)) {
                pos2 = iter;
                changed = true;
            }
            if (changed && pos1 != -1 && pos2 != -1) {
                len = Math.min(Math.abs(pos1 - pos2), len);
                changed = false;
            }
            iter++;
        }
        return len;
    }
}
{% endhighlight %}