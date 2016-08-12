---
layout: post
title: Leetcode Problem 293 Summary
date: 2016-08-12
categories: blog
tags: [study]

---

# 题目

You are playing the following Flip Game with your friend: Given a string that contains only these two characters: + and -, you and your friend take turns to flip two consecutive "++" into "--". The game ends when a person can no longer make a move and therefore the other person will be the winner.

Write a function to compute all possible states of the string after one valid move.

For example, given s = "++++", after one move, it may become one of the following states:

[  
  "--++",  
  "+--+",  
  "++--"  
]  
If there is no valid move, return an empty list [].

# 我的算法

遍历到 ++ 反转成 -- 即可。

# 代码

{% highlight java %}
public class Solution {
    public List<String> generatePossibleNextMoves(String s) {
        if (s == null || s.length() < 2) return new ArrayList<>();
        
        char[] str = s.toCharArray();
        List<String> ret = new ArrayList<>();
        for (int i = 0; i < str.length - 1; i++) {
            if (str[i] == str[i + 1] && str[i] == '+') {
                str[i] = str[i + 1] = '-';
                ret.add(new String(str));
                str[i] = str[i + 1] = '+';
            }
        }
        
        return ret;
    }
}
{% endhighlight %}