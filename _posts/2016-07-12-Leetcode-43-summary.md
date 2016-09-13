---
layout: post
title: Leetcode Problem 43 Summary
date: 2016-07-12
categories: blog
tags: [study]

---

# 题目

Given two numbers represented as strings, return multiplication of the numbers as a string.

Note:

* The numbers can be arbitrarily large and are non-negative.
* Converting the input string to integer is NOT allowed.
* You should NOT use internal library such as BigInteger.

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

# 二刷

{% highlight java %}
public class Solution {
    public String multiply(String num1, String num2) {
        int m = num1.length(), n = num2.length();
        String[] halfResult = new String[n];
        for (int i = n - 1; i >= 0; i--) {
            int index = n - 1 - i;
            halfResult[index] = unitMulti(num1, num2.charAt(i));
        }
        String result = halfResult[0];
        for (int i = 1; i < n; i++) {
            result = unitPlus(result, halfResult[i], i);
        }
        
        int start = 0;
        while (start < result.length() && result.charAt(start) == '0') start++;
        if (start == result.length()) return "0";
        else return result.substring(start);
    }
    
    private String unitMulti(String num1, char num2) {
        int carry = 0;
        int digit2 = (int) (num2 - '0');
        StringBuilder sb = new StringBuilder();
        for (int i = num1.length() - 1; i >= 0; i--) {
            int digit = (int) (num1.charAt(i) - '0');
            digit = digit * digit2 + carry;
            carry = digit / 10;
            digit = digit % 10;
            sb.insert(0, digit);
        }
        if (carry != 0) sb.insert(0, carry);
        return sb.toString();
    }
    
    private String unitPlus(String num1, String num2, int shift) {
        int carry = 0;
        StringBuilder sb = new StringBuilder();
        StringBuilder number2 = new StringBuilder(num2);
        for (int i = 0; i < shift; i++) {
            number2.append(0);
        }
        num2 = number2.toString();
        
        int p1 = num1.length() - 1;
        int p2 = num2.length() - 1;
        while (p1 >= 0 || p2 >= 0) {
            if (p1 < 0) {
                int digit2 = (int) (num2.charAt(p2) - '0');
                digit2 += carry;
                carry = digit2 / 10;
                digit2 = digit2 % 10;
                sb.insert(0, digit2);
                p2--;
            } else if (p2 < 0) {
                int digit1 = (int) (num1.charAt(p1) - '0');
                digit1 += carry;
                carry = digit1 / 10;
                digit1 = digit1 % 10;
                sb.insert(0, digit1);
                p1--;
            } else {
                int digit1 = (int) (num1.charAt(p1) - '0');
                int digit2 = (int) (num2.charAt(p2) - '0');
                int digit = digit1 + digit2 + carry;
                carry = digit / 10;
                digit = digit % 10;
                sb.insert(0, digit);
                p1--;
                p2--;
            }
        }
        if (carry != 0) sb.insert(0, carry);
        return sb.toString();
    }
}
{% endhighlight %}

# discuss

{% highlight java %}
public String multiply(String num1, String num2) {
    int m = num1.length(), n = num2.length();
    int[] pos = new int[m + n];
   
    for(int i = m - 1; i >= 0; i--) {
        for(int j = n - 1; j >= 0; j--) {
            int mul = (num1.charAt(i) - '0') * (num2.charAt(j) - '0'); 
            int p1 = i + j, p2 = i + j + 1;
            int sum = mul + pos[p2];

            pos[p1] += sum / 10;
            pos[p2] = (sum) % 10;
        }
    }  
    
    StringBuilder sb = new StringBuilder();
    for(int p : pos) if(!(sb.length() == 0 && p == 0)) sb.append(p);
    return sb.length() == 0 ? "0" : sb.toString();
}
{% endhighlight %}