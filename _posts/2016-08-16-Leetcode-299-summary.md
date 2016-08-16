---
layout: post
title: Leetcode Problem 299 Summary
date: 2016-08-16
categories: blog
tags: [study]

---

# 题目

You are playing the following Bulls and Cows game with your friend: You write down a number and ask your friend to guess what the number is. Each time your friend makes a guess, you provide a hint that indicates how many digits in said guess match your secret number exactly in both digit and position (called "bulls") and how many digits match the secret number but locate in the wrong position (called "cows"). Your friend will use successive guesses and hints to eventually derive the secret number.

For example:

Secret number:  "1807"  
Friend's guess: "7810"  

Hint: 1 bull and 3 cows. (The bull is 8, the cows are 0, 1 and 7.)

Write a function to return a hint according to the secret number and friend's guess, use A to indicate the bulls and B to indicate the cows. In the above example, your function should return "1A3B".

Please note that both secret number and friend's guess may contain duplicate digits, for example:

Secret number:  "1123"  
Friend's guess: "0111"  

In this case, the 1st 1 in friend's guess is a bull, the 2nd or 3rd 1 is a cow, and your function should return "1A1B".  

You may assume that the secret number and your friend's guess only contain digits, and their lengths are always equal.

# 我的算法

对于相同位置相同数字，遍历时可以O(1)发现，对于不同位置相同数字，遍历时不能O(1)发现，于是想到哈希表把数字和数量做个映射。

后期发现因为是数字和数量的映射关系，用计数排序的思路不需要哈希表，用数组就可以，会快一点。

# 代码

{% highlight java %}
public class Solution {
    public String getHint(String secret, String guess) {
        int bull = 0;
        int cow = 0;
        char[] counter = new char[10];    

        for (int i = 0; i < secret.length(); i++) {
            if (guess.charAt(i) == secret.charAt(i)) {
                bull++;
            } else {
                counter[(int)(secret.charAt(i) - '0')]++;
            }
        }
        
        for (int i = 0; i < guess.length(); i++) {
            if (guess.charAt(i) != secret.charAt(i) && counter[(int)(guess.charAt(i) - '0')] > 0) {
                cow++;
                counter[(int)(guess.charAt(i) - '0')]--;
            }
        }
        
        String ret = bull + "A" + cow + "B";
        return ret;
    }
}
{% endhighlight %}