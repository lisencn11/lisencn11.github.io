---
layout: post
title: Leetcode Problem 415 Summary
date: 2016-10-15
categories: blog
tags: [study]

---

# 题目

Given two non-negative numbers num1 and num2 represented as string, return the sum of num1 and num2.

Note:

1. The length of both num1 and num2 is < 5100.
2. Both num1 and num2 contains only digits 0-9.
3. Both num1 and num2 does not contain any leading zero.
4. You must not use any built-in BigInteger library or convert the inputs to integer directly.

# 我的算法

少用StringBuilder.insert()，复杂度不是O(1)。

# 代码

{% highlight java %}
public class Solution {
    public String addStrings(String num1, String num2) {
        int carry = 0;
        int index1 = num1.length() - 1, index2 = num2.length() - 1;
        StringBuilder result = new StringBuilder();
        
        while (index1 >= 0 || index2 >= 0) {
            int digit1 = (index1 >= 0) ? (int) (num1.charAt(index1) - '0') : 0;
            int digit2 = (index2 >= 0) ? (int) (num2.charAt(index2) - '0') : 0;
            int sum = digit1 + digit2 + carry;
            carry = sum / 10;
            sum = sum % 10;
            result.append(sum);
            index1--;
            index2--;
        }
        if (carry == 1) {
            result.append(1);
        }
        
        return result.reverse().toString();
    }
}
{% endhighlight %}