---
layout: post
title: Leetcode Problem 43 Summary
date: 2016-07-12
categories: blog
tags: [study]

---

# 题目

**输入**两个String类型表示的正整型。

**输出**两数相乘结果。

**要求**两个数任意大，不能将其转为int，不能使用BigInteger。

# 我的算法

模拟两数相乘的过程即可，对于两个数每位按位乘后相加，注意处理进位情况。

# 代码

{% highlight java %}
public class Solution {
    public String multiply(String num1, String num2) {
        if (num1 == null || num1.length() == 0 || num2 == null || num2.length() == 0) {
            return "0";
        }
        
        char[] number1 = num1.toCharArray();
        char[] number2 = num2.toCharArray();
        int len1 = number1.length;
        int len2 = number2.length;
        char[] result = new char[len1 + len2 + 1];
        int resLen = result.length;
        char[] tempResult = new char[len2 + 1];
        int carry = 0;
        int product = 0;
        int remain = 0;
        int sum = 0;
        
        for (int i = 0; i < result.length; i++) {
            result[i] = '0';
        }
        
        for (int i = len1 - 1; i >= 0; i--) {
            int temp1 = number1[i] - '0';
            for (int j = len2 - 1; j >= 0; j--) {
                int temp2 = number2[j] - '0';
                product = temp1 * temp2 + carry;
                remain = product % 10;
                carry = product / 10;
                tempResult[j + 1] = (char)(remain + '0');
            }
            tempResult[0] = (char)(carry + '0');
            carry = 0;
            
            int trans = len1 - 1 - i;
            for (int j = 0; j <= len2; j++) {
                int index = resLen - 1 - trans - j;
                temp1 = tempResult[len2 - j] - '0';
                int temp2 = result[index] - '0';
                sum = temp1 + temp2 + carry;
                remain = sum % 10;
                carry = sum / 10;
                result[index] = (char)(remain + '0');
            }
            if (carry != 0) {
                result[resLen - 1 - trans - len2 - 1] = (char)(carry + '0');
            }
        }
        
        StringBuilder sb = new StringBuilder();
        boolean firstZero = true;
        for (int i = 0; i < resLen; i++) {
            if (firstZero && result[i] == '0') {
                continue;
            } else if (firstZero) {
                firstZero = false;
                sb.append(result[i]);
            } else {
                sb.append(result[i]);
            }
        }
        if (firstZero) {
            return "0";
        }
        
        return sb.toString();
    }
}
{% endhighlight %}