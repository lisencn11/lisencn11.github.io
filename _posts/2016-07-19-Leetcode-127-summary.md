---
layout: post
title: Leetcode Problem 127 Summary
date: 2016-07-19
categories: blog
tags: [study]

---

# 题目

字符串由 a-z 组成，给出一个起点字符串，一个终点字符串，一个中间点字符串集合Set，字符串每次只能变一个字符，问最短经过多少步，可以从起点字符串，通过中间点字符串集合中的元素，变成终点字符串。如果不能则返回0。

例如：

>Given:  
beginWord = "hit"  
endWord = "cog"  
wordList = ["hot","dot","dog","lot","log"]  
As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",  
return its length 5.

所有字符串长度相等，且均只包含小写字母。

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