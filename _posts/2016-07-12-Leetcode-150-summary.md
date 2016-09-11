---
layout: post
title: Leetcode Problem 150 Summary
date: 2016-07-12
categories: blog
tags: [study]

---

# 题目

Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are +, -, *, /. Each operand may be an integer or another expression.

Some examples:  
  ["2", "1", "+", "3", "*"] -> ((2 + 1) * 3) -> 9  
  ["4", "13", "5", "/", "+"] -> (4 + (13 / 5)) -> 6

# 我的算法

从前到后遍历数组，遇到数字压栈，遇到操作符弹出两个数进行操作后将结果压栈。

# 代码

{% highlight java %}
public class Solution {
    public int evalRPN(String[] tokens) {
        if (tokens == null || tokens.length == 0) {
            return 0;
        }
        
        int ret = 0;
        Stack<Integer> stack = new Stack<>();
        for (String str : tokens) {
            if (str.equals("+")) {
                Integer op2 = stack.pop();
                Integer op1 = stack.pop();
                Integer res = op1 + op2;
                stack.push(res);
            } else if (str.equals("-")) {
                Integer op2 = stack.pop();
                Integer op1 = stack.pop();
                Integer res = op1 - op2;
                stack.push(res);
            } else if (str.equals("*")) {
                Integer op2 = stack.pop();
                Integer op1 = stack.pop();
                Integer res = op1 * op2;
                stack.push(res);
            } else if (str.equals("/")) {
                Integer op2 = stack.pop();
                Integer op1 = stack.pop();
                Integer res = op1 / op2;
                stack.push(res);
            } else {
                Integer temp = Integer.parseInt(str);
                stack.push(temp);
            }
        }
        ret = stack.pop();
        return ret;
    }
}
{% endhighlight %}

# 二刷

{% highlight java %}
public class Solution {
    public int evalRPN(String[] tokens) {
        if (tokens == null || tokens.length == 0) return 0;
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < tokens.length; i++) {
            if (tokens[i].charAt(tokens[i].length() - 1) <= '9' && tokens[i].charAt(tokens[i].length() - 1) >= '0') {
                stack.push(Integer.parseInt(tokens[i]));
            } else {
                int num2 = stack.pop();
                int num1 = stack.pop();
                if (tokens[i].charAt(0) == '+') {
                    stack.push(num1 + num2);
                } else if (tokens[i].charAt(0) == '-') {
                    stack.push(num1 - num2);
                } else if (tokens[i].charAt(0) == '*') {
                    stack.push(num1 * num2);
                } else {
                    stack.push(num1 / num2);
                }
            }
        }
        return stack.pop();
    }
}
{% endhighlight %}