---
layout: post
title: Leetcode Problem 273 Summary
date: 2016-10-04
categories: blog
tags: [study]

---

# 题目

Convert a non-negative integer to its english words representation. Given input is guaranteed to be less than 231 - 1.

For example,

>123 -> "One Hundred Twenty Three"  
12345 -> "Twelve Thousand Three Hundred Forty Five"  
1234567 -> "One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven"

Hint:

1. Did you see a pattern in dividing the number into chunk of words? For example, 123 and 123000.
2. Group the number by thousands (3 digits). You can write a helper function that takes a number less than 1000 and convert just that chunk to words.
3. There are many edge cases. What are some good test cases? Does your code work with input such as 0? Or 1000010? (middle chunk is zero and should not be printed out)

# 我的算法

每次取出三位进行处理。

# 代码

{% highlight java %}
public class Solution {
    String[] units = new String[]{" One", " Two", " Three", " Four", " Five", " Six", " Seven", " Eight", " Nine"};
    String[] tens = new String[]{" Ten", " Eleven", " Twelve", " Thirteen", " Fourteen", " Fifteen", " Sixteen", " Seventeen", " Eighteen", " Nineteen"};
    String[] tenthUnits = new String[]{" Twenty", " Thirty", " Forty", " Fifty", " Sixty", " Seventy", " Eighty", " Ninety"};
    String[] thousandth = new String[]{"", " Thousand", " Million", " Billion"};
    
    public String numberToWords(int num) {
        if (num == 0) return "Zero";
        StringBuilder sb = new StringBuilder();
        int thouIndex = 0;

        while (num > 0) {
            int remain = num % 1000;
            String words = translate(remain);
            if (!words.equals("Zero")) {
                sb.insert(0, words + thousandth[thouIndex]);
            }
            thouIndex++;
            num /= 1000;
        }
        return sb.substring(1);
    }
    
    private String translate(int number) {
        if (number == 0) return "Zero";
        int hundred = number / 100;
        int ten = (number % 100) / 10;
        int unit = number % 10;
        StringBuilder sb = new StringBuilder();
        if (hundred != 0) {
            sb.append(units[hundred - 1]);
            sb.append(" Hundred");
        }
        if (ten == 1) {
            sb.append(tens[unit]);
        } else if (ten != 0) {
            sb.append(tenthUnits[ten - 2]);
        }
        if (unit != 0 && ten != 1) {
            sb.append(units[unit - 1]);
        }
        return sb.toString();
    } 
}
{% endhighlight %}