---
layout: post
title: Leetcode Problem 260 Summary
date: 2016-08-30
categories: blog
tags: [study]

---

# 题目

A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Find all strobogrammatic numbers that are of length = n.

For example,  
Given n = 2, return ["11","69","88","96"].

Hint:

Try to use recursion and notice that it should recurse with n - 2 instead of n - 1.

# 我的算法

似乎 recursion 比较简单，我是用 iterative 做的，注意 0 的处理和 n 为奇数的处理即可。

# 代码

{% highlight java %}
public class Solution {
    public List<String> findStrobogrammatic(int n) {
        if (n == 0) return new ArrayList<>();
        List<Pair> map = new ArrayList<>();
        map.add(new Pair("0", "0"));
        map.add(new Pair("1", "1"));
        map.add(new Pair("6", "9"));
        map.add(new Pair("8", "8"));
        map.add(new Pair("9", "6"));
        List<String> list = new ArrayList<>();
        if (n % 2 == 1) {
            list.add("0");
            list.add("1");
            list.add("8");
            n--;
        } else {
            list.add("");
        }
        
        for (int i = 1; i <= n / 2; i++) {
            List<String> temp = new ArrayList<>();
            for (String pre : list) {
                for (Pair pair : map) {
                    if (i == n / 2 && pair.front.equals("0")) continue;
                    String s = pair.front + pre + pair.end;
                    temp.add(s);
                }
            }
            list = temp;
        }
        return list;
    }
    
    class Pair {
        String front;
        String end;
        public Pair(String front, String end) {
            this.front = front;
            this.end = end;
        }
    }
}
{% endhighlight %}