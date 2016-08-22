---
layout: post
title: Leetcode Problem 294 Summary
date: 2016-08-21
categories: blog
tags: [study]

---

# 题目

You are playing the following Flip Game with your friend: Given a string that contains only these two characters: + and -, you and your friend take turns to flip two consecutive "++" into "--". The game ends when a person can no longer make a move and therefore the other person will be the winner.

Write a function to determine if the starting player can guarantee a win.

For example, given s = "++++", return true. The starting player can guarantee a win by flipping the middle "++" to become "+--+".

Follow up:  
Derive your algorithm's runtime complexity.

# 我的算法

我尝试用动态规划算法去解决，但是没有成功。

discuss中有使用递归来模拟所有情况的解法和动态规划的解法，这里暂时只贴上递归解法，动态规划解法较复杂，贴在[这里](https://discuss.leetcode.com/topic/27282/theory-matters-from-backtracking-128ms-to-dp-0ms/2)

# 代码

{% highlight java %}
public class Solution {
    public boolean canWin(String s) {
        char[] str = s.toCharArray();
        return canWin(str);
    }
    
    private boolean canWin(char[] str) {
        for (int i = 0; i < str.length - 1; i++) {
            if (str[i] == '+' && str[i + 1] == '+') {
                str[i] = '-';
                str[i + 1] = '-';
                boolean win = !canWin(str);
                str[i] = '+';
                str[i + 1] = '+';
                if (win) return true;
            }
        }
        return false;
    }
}
{% endhighlight %}