---
layout: post
title: Leetcode Problem 67 Summary
date: 2016-08-18
categories: blog
tags: [study]

---

# 题目

Given two binary strings, return their sum (also a binary string).

For example,  
a = "11"  
b = "1"  
Return "100".

# 我的算法

没什么算法考点，对于边界条件的处理觉得 discuss 中的很好，附在下方。

# 代码

{% highlight java %}
public class Solution {
    public String addBinary(String a, String b) {
        StringBuilder result = new StringBuilder();
        int carry = 0;
        
        int pA = a.length() - 1;
        int pB = b.length() - 1;
        
        while (pA >=0 && pB >= 0) {
            int numA = Character.getNumericValue(a.charAt(pA));
            int numB = Character.getNumericValue(b.charAt(pB));
            int sum = numA + numB + carry;
            carry = sum / 2;
            int remain = sum % 2;
            result.insert(0, remain);
            pA--;
            pB--;
        }
        
        while (pA >= 0) {
            int numA = Character.getNumericValue(a.charAt(pA));
            int sum = numA + carry;
            carry = sum / 2;
            int remain = sum % 2;
            result.insert(0, remain);
            pA--;
        }
        
        while (pB >= 0) {
            int numB = Character.getNumericValue(b.charAt(pB));
            int sum = numB + carry;
            carry = sum / 2;
            int remain = sum % 2;
            result.insert(0, remain);
            pB--;
        }
        
        if (carry == 1) result.insert(0, "1");
        return result.toString();
    }
}
{% endhighlight %}

# discuss

{% highlight java %}
public class Solution {
    public String addBinary(String a, String b) {
        if(a == null || a.isEmpty()) {
            return b;
        }
        if(b == null || b.isEmpty()) {
            return a;
        }
        char[] aArray = a.toCharArray();
        char[] bArray = b.toCharArray();
        StringBuilder stb = new StringBuilder();

        int i = aArray.length - 1;
        int j = bArray.length - 1;
        int aByte;
        int bByte;
        int carry = 0;
        int result;

        while(i > -1 || j > -1 || carry == 1) {
            aByte = (i > -1) ? Character.getNumericValue(aArray[i--]) : 0;
            bByte = (j > -1) ? Character.getNumericValue(bArray[j--]) : 0;
            result = aByte ^ bByte ^ carry;
            carry = ((aByte + bByte + carry) >= 2) ? 1 : 0;
            stb.append(result);
        }
        return stb.reverse().toString();
    }
}
{% endhighlight %}