---
layout: post
title: Leetcode Problem 127 Summary
date: 2016-07-19
categories: blog
tags: [study]

---

# 题目

Given two words (beginWord and endWord), and a dictionary's word list, find the length of shortest transformation sequence from beginWord to endWord, such that:

1. Only one letter can be changed at a time
2. Each intermediate word must exist in the word list
For example,

Given:  
beginWord = "hit"  
endWord = "cog"  
wordList = ["hot","dot","dog","lot","log"]

As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",  
return its length 5.

Note:

* Return 0 if there is no such transformation sequence.
* All words have the same length.
* All words contain only lowercase alphabetic characters.

# 我的算法

初步想法是BFS，先遍历距离（不同字符数）为 1 的所有字符串，直到遍历到终点字符串为止，层数即为所要求返回值。

但是初步想法的问题是，对一个指定的字符串，从集合中找它距离为 1 的字符串的复杂度比较高，会导致超时。

参考discuss中的题解，看出对于当前字符串，遍历所有距离为 1 的字符串可能组合的复杂度甚至小于从集合中求距离为 1 的字符串，于是改用这种暴力方法后就不超时了。

# 代码

{% highlight java %}
public class Solution {
    public int ladderLength(String beginWord, String endWord, Set<String> wordList) {
        Queue<String> sameDistance = new LinkedList<>();
        sameDistance.offer(beginWord);
        wordList.add(endWord);
        
        int counter = 1;
        while (!sameDistance.isEmpty()) {
            for (int i = sameDistance.size() - 1; i >= 0; i--) {
                String curr = sameDistance.poll();
                
                for (int j = 0; j < curr.length(); j++) {
                    char[] temp = curr.toCharArray();
                    for (char k = 'a'; k <= 'z'; k++) {
                        temp[j] = k;
                        String tempCurr = new String(temp);
                        if (wordList.contains(tempCurr)) {
                            if (tempCurr.equals(endWord)) return counter + 1;
                            wordList.remove(tempCurr);
                            sameDistance.offer(tempCurr);
                        }
                    }
                }
            }
            counter++;
        }
        
        return 0;
    }
}
{% endhighlight %}

# 二刷

{% highlight java %}
public class Solution {
    public int ladderLength(String beginWord, String endWord, Set<String> wordList) {
        Queue<String> queue = new LinkedList<>();
        queue.offer(beginWord);
        wordList.remove(beginWord);
        int count = 1;

        while (!queue.isEmpty()) {
            int len = queue.size();
            count++;
            for (int i = 0; i < len; i++) {
                String curr = queue.poll();
                for (int j = 0; j < curr.length(); j++) {
                    StringBuilder sb = new StringBuilder(curr);
                    for (char ch = 'a'; ch <= 'z'; ch++) {
                        sb.setCharAt(j, ch);
                        String neighbor = sb.toString();
                        if (wordList.contains(neighbor)) {
                            if (neighbor.equals(endWord)) return count;
                            queue.offer(neighbor);
                            wordList.remove(neighbor);
                        }
                    }
                }
            }
        }
        return 0;
    }
}
{% endhighlight %}