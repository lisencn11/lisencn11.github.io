---
layout: post
title: Leetcode Problem 6 Summary
date: 2016-08-18
categories: blog
tags: [study]

---

# 题目

The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

![problem6](https://lisencn11.github.io/img/problem6.png)

And then read line by line: "PAHNAPLSIIGYIR"

Write the code that will take a string and make this conversion given a number of rows:

string convert(string text, int nRows);

convert("PAYPALISHIRING", 3) should return "PAHNAPLSIIGYIR".

# 我的算法

对每一行创建一个 StringBuilder 模拟 zigzag的顺序挨个加入。

# 代码

{% highlight java %}
public class Solution {
    public String convert(String s, int numRows) {
        StringBuilder[] convert = new StringBuilder[numRows];
        for (int i = 0; i < numRows; i++) convert[i] = new StringBuilder();
        
        int iter = 0;
        while (iter < s.length()) {
            for (int i = 0; i < numRows && iter < s.length(); i++) {
                convert[i].append(s.charAt(iter++));
            }
            
            for (int i = numRows - 2; i > 0  && iter < s.length(); i--) {
                convert[i].append(s.charAt(iter++));
            }
        }
        
        StringBuilder ret = new StringBuilder();
        for (int i = 0; i < convert.length; i++)
            ret.append(convert[i]);
            
        return ret.toString();
    }
}
{% endhighlight %}