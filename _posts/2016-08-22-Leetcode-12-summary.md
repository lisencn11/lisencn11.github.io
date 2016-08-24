---
layout: post
title: Leetcode Problem 12 Summary
date: 2016-08-22
categories: blog
tags: [study]

---

# 题目

Given an integer, convert it to a roman numeral.

Input is guaranteed to be within the range from 1 to 3999.

# 我的算法

主要考虑罗马数字规则。

# 代码

{% highlight java %}
public class Solution {
    public String intToRoman(int num) {
        String[] roman = {"I", "IV", "V", "IX", "X", "XL", "L", "XC", "C", "CD", "D", "CM", "M"};
        int[] number = {1, 4, 5, 9, 10, 40, 50, 90, 100, 400, 500, 900, 1000};
        StringBuilder ret = new StringBuilder();
        int remain = num;
        
        for (int i = number.length - 1; i >= 0; i--) {
            while (remain >= number[i]) {
                remain -= number[i];
                ret.append(roman[i]);
            }
            if (remain == 0) break;
        }
        
        return ret.toString();
    }
}
{% endhighlight %}