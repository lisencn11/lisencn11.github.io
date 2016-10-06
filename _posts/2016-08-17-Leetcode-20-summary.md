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

# 二刷

注意，if 语句中条件过长应：

1. 在与或连接符前短剧
2. 采用8空格缩进，以便看清 body

{% highlight java %}
public class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        
        for (int i = 0; i < s.length(); i++) {
            char curr = s.charAt(i);
            if (curr == '(' || curr == '[' || curr == '{') {
                stack.push(curr);
            } else if (stack.isEmpty()) {
                return false;
            } else if ((curr == ')' && stack.peek() == '(') 
                    || (curr == ']' && stack.peek() == '[') 
                    || (curr == '}' && stack.peek() == '{')) {
                stack.pop();
            } else {
                return false;
            }
        }
        
        return stack.isEmpty();
    }
}
{% endhighlight %}