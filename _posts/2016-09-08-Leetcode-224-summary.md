---
layout: post
title: Leetcode Problem 224 Summary
date: 2016-09-08
categories: blog
tags: [study]

---

# 题目

Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open ( and closing parentheses ), the plus + or minus sign -, non-negative integers and empty spaces .

You may assume that the given expression is always valid.

Some examples:  
"1 + 1" = 2  
" 2-1 + 2 " = 3  
"(1+(4+5+2)-3)+(6+8)" = 23  
Note: Do not use the eval built-in library function.

# 我的算法

用 Stack ，遇到左括号将之前的结果入栈，并将前面的运算符也入栈，遇到右括号则出栈，并提取之前的运算符进行运算。

# 代码

{% highlight java %}
public class Solution {
    public int calculate(String s) {
        s = s.replaceAll(" ", "");
        if (s.length() == 0) return 0;
        Stack<Integer> cache = new Stack<>();
        Stack<Boolean> plus = new Stack<>();
        int index = 0;
        int pre = -1;
        int result = 0;
        boolean prePlus = true; 
        while (index < s.length()) {
            if (s.charAt(index) == '(') {
                cache.push(result);
                plus.push(prePlus);
                result = 0;
                pre = index;
                prePlus = true;
            } else if (s.charAt(index) == ')') {
                if (pre < index - 1) {
                    int number = Integer.parseInt(s.substring(pre + 1, index));
                    if (prePlus) result += number;
                    else result -= number;
                }
                int preResult = cache.pop();
                prePlus = plus.pop();
                if (prePlus) result = preResult + result;
                else result = preResult - result;
                pre = index;
                prePlus = true;
            } else if (s.charAt(index) == '+') {
                if (pre == index - 1) {
                    prePlus = true;
                    pre = index;
                } else {
                    int number = Integer.parseInt(s.substring(pre + 1, index));
                    if (prePlus) result += number;
                    else result -= number;
                    prePlus = true;
                    pre = index;
                }
            } else if (s.charAt(index) == '-') {
                if (pre == index - 1) {
                    prePlus = false;
                    pre = index;
                } else {
                    int number = Integer.parseInt(s.substring(pre + 1, index));
                    if (prePlus) result += number;
                    else result -= number;
                    prePlus = false;
                    pre = index;
                }
            }
            index++;
        }
        
        if (s.charAt(s.length() - 1) != ')') {
            int number = Integer.parseInt(s.substring(pre + 1, s.length()));
            if (prePlus) result += number;
            else result -= number;
        }
        return result;
    }
}
{% endhighlight %}