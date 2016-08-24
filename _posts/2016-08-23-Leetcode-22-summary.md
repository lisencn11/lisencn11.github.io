---
layout: post
title: Leetcode Problem 22 Summary
date: 2016-08-23
categories: blog
tags: [study]

---

# 题目

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given n = 3, a solution set is:

[  
  "((()))",  
  "(()())",  
  "(())()",  
  "()(())",  
  "()()()"  
]

# 我的算法

递归算法，传递参数包括：当前结果，当前位置，多余的左括号。左括号只有在多余的左括号少于剩余空间的时候才能加入，右括号只有在存在多余左括号的时候才能加入。

# 代码

{% highlight java %}
public class Solution {
    List<String> result = new ArrayList<>();
    public List<String> generateParenthesis(int n) {
        char[] string = new char[n * 2];
        helper(string, 0, 0);
        
        return result;
    }
    
    private void helper(char[] string, int leftCount, int start) {
        if (start == string.length && leftCount == 0) {
            String s = new String(string);
            result.add(s);
        }
        
        if (leftCount > 0) {
            string[start] = ')';
            helper(string, leftCount - 1, start + 1);
        }
        
        if (leftCount < string.length - start) {
            string[start] = '(';
            helper(string, leftCount + 1, start + 1);
        }
    }
}
{% endhighlight %}