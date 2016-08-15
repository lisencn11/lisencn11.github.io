---
layout: post
title: Leetcode Problem 13 Summary
date: 2016-08-14
categories: blog
tags: [study]

---

# 题目

Given a roman numeral, convert it to an integer.

Input is guaranteed to be within the range from 1 to 3999.

# 我的算法

罗马数字转化为整型，主要考察罗马数字规则？没什么意思的一道题。

基本规则有字母数字对应关系，然后就是除了下面几种情况直接相加即可。

* I placed before V or X indicates one less, so four is IV (one less than five) and nine is IX (one less than ten)
* X placed before L or C indicates ten less, so forty is XL (ten less than fifty) and ninety is XC (ten less than a hundred)
* C placed before D or M indicates a hundred less, so four hundred is CD (a hundred less than five hundred) and nine hundred is CM (a hundred less than a thousand)

# 代码

{% highlight java %}
public class Solution {
    public int romanToInt(String s) {
        Map<Character, Integer> map = new HashMap<>();
        map.put('I', 1);
        map.put('X', 10);
        map.put('C', 100);
        map.put('M', 1000);
        map.put('V', 5);
        map.put('L', 50);
        map.put('D', 500);
        
        int sum = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == 'I' && i < s.length() - 1 && (s.charAt(i + 1) == 'V' || s.charAt(i + 1) == 'X')) {
                sum -= map.get('I');
            } else if (s.charAt(i) == 'X' && i < s.length() - 1 && (s.charAt(i + 1) == 'L' || s.charAt(i + 1) == 'C')) {
                sum -= map.get('X');
            } else if (s.charAt(i) == 'C' && i < s.length() - 1 && (s.charAt(i + 1) == 'M' || s.charAt(i + 1) == 'D')) {
                sum -= map.get('C');
            } else {
                sum += map.get(s.charAt(i));
            }
        }
        
        return sum;
    }
}
{% endhighlight %}