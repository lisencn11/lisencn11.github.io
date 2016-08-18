---
layout: post
title: Leetcode Problem 20 Summary
date: 2016-08-17
categories: blog
tags: [study]

---

# 题目

Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

The brackets must close in the correct order, "()" and "()[]{}" are all valid but "(]" and "([)]" are not.

# 我的算法

用 Stack 存左半边的括号，遇到右半边就出栈检查是否是一对。

# 代码

{% highlight java %}
public class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(' || s.charAt(i) == '[' || s.charAt(i) == '{') {
                stack.push(s.charAt(i));
            } else if (s.charAt(i) == ')') {
                if (stack.isEmpty() || stack.pop() != '(') return false;
            } else if (s.charAt(i) == ']') {
                if (stack.isEmpty() || stack.pop() != '[') return false;
            } else if (s.charAt(i) == '}') {
                if (stack.isEmpty() || stack.pop() != '{') return false;
            }
        }
        
        return stack.isEmpty();
    }
}
{% endhighlight %}