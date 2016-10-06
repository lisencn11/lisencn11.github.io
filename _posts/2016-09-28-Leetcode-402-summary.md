---
layout: post
title: Leetcode Problem 401 Summary
date: 2016-09-28
categories: blog
tags: [study]

---

# 题目

Given a non-negative integer num represented as a string, remove k digits from the number so that the new number is the smallest possible.

Note:

* The length of num is less than 10002 and will be ≥ k.
* The given num does not contain any leading zero.

Example 1:

Input: num = "1432219", k = 3  
Output: "1219"  
Explanation: Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.

Example 2:

Input: num = "10200", k = 1  
Output: "200"  
Explanation: Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes.

Example 3:

Input: num = "10", k = 2  
Output: "0"  
Explanation: Remove all the digits from the number and it is left with nothing which is 0.

# 我的算法

思路很直接，如果发现一位比前一位小，且有剩余的 remove 量，那么 remove 前一位。

# 代码

{% highlight java %}
public class Solution {
    public String removeKdigits(String num, int k) {
        if (k >= num.length()) return "0";
        
        Stack<Character> stack = new Stack<>();
        StringBuilder result = new StringBuilder();
        int remain = k, start = 0, expLen = num.length() - k;
        
        stack.push(num.charAt(0));
        for (int i = 1; i < num.length(); i++) {
            char curr = num.charAt(i);
            if (remain > 0 && curr < stack.peek()) {
                while (remain > 0 && !stack.isEmpty() && curr < stack.peek()) {
                    stack.pop();
                    remain--;
                }
                stack.push(curr);
            } else {
                stack.push(curr);
            }
        }
        
        while (!stack.isEmpty()) {
            result.append(stack.pop());
        }
        result.reverse();
        while (start < result.length() && result.charAt(start) == '0') start++;
        if (start == result.length()) return "0";
        else return result.substring(start, Math.min(result.length(), start + expLen));
    }
}
{% endhighlight %}