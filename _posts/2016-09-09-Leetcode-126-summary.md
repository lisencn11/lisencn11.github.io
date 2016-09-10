---
layout: post
title: Leetcode Problem 126 Summary
date: 2016-09-09
categories: blog
tags: [study]

---

# 题目

Given two words (beginWord and endWord), and a dictionary's word list, find all shortest transformation sequence(s) from beginWord to endWord, such that:

1. Only one letter can be changed at a time
2. Each intermediate word must exist in the word list
For example,

Given:  
beginWord = "hit"  
endWord = "cog"  
wordList = ["hot","dot","dog","lot","log"]  
Return  

  [  
    ["hit","hot","dot","dog","cog"],  
    ["hit","hot","lot","log","cog"]  
  ]
  
Note:

* All words have the same length.  
* All words contain only lowercase alphabetic characters.

# 我的算法

BFS算法，当前节点为一个字符串，从wordList中查找下一个符合条件的字符串，这里需要注意的是，我一开始是对两个字符串挨个字符进行比较，但是这样对比较大的wordList会很慢，更快的做法是对当前word的每一位字符从 'a' 到 'z' 进行修改并 check 是否在 HashSet 中。

# 代码

{% highlight java %}
public class Solution {
    public List<List<String>> findLadders(String beginWord, String endWord, Set<String> wordList) {
        List<List<String>> result = new ArrayList<>();
        Queue<List<String>> paths = new LinkedList<>();
        List<String> path = new ArrayList<>();
        path.add(beginWord);
        paths.offer(path);
        boolean reachEnd = false;
        wordList.remove(beginWord);
        
        while (!reachEnd && !paths.isEmpty()) {
            int len = paths.size();
            Set<String> currCand = new HashSet<>(wordList);
            for (int i = 0; i < len; i++) {
                List<String> currPath = paths.poll();
                String end = currPath.get(currPath.size() - 1);
                for (int j = 0; j < end.length(); j++) {
                    StringBuilder sb = new StringBuilder(end);
                    for (char ch = 'a'; ch <= 'z'; ch++) {
                        sb.setCharAt(j, ch);
                        String currNode = sb.toString();
                        if (currCand.contains(currNode)) {
                            if (currNode.equals(endWord)) {
                                List<String> newPath = new ArrayList<>(currPath);
                                newPath.add(currNode);
                                result.add(newPath);
                                reachEnd = true;
                            } else if (!reachEnd) {
                                List<String> newPath = new ArrayList<>(currPath);
                                newPath.add(currNode);
                                paths.offer(newPath);
                                if (wordList.contains(currNode)) wordList.remove(currNode);
                            }
                        }
                    }
                }
            }
        }
        
        return result;
    }
    
    private int distance(String s1, String s2) {
        int len = s1.length(), count = 0;
        for (int i = 0; i < len; i++) {
            count = s1.charAt(i) != s2.charAt(i) ? count + 1 : count;
        }
        return count;
    }
}
{% endhighlight %}