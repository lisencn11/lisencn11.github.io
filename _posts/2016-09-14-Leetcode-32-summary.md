---
layout: post
title: Leetcode Problem 32 Summary
date: 2016-09-14
categories: blog
tags: [study]

---

# 题目

Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.

For "(()", the longest valid parentheses substring is "()", which has length = 2.

Another example is ")()())", where the longest valid parentheses substring is "()()", which has length = 4.

# 我的算法

DP。 我的算法是用一个数组存储当前位置结尾的有效长度：

1. 对于 ( 而言为0
2. 对于 ) ，如果它左边是 ( 则，result[i] = result[i - 2] + 2
3. 对于 ) ，如果它左边是 ) 则，result[i] = result[i - 1] + 2 result[i] += result[i - result[i]];

评论里用 Stack 算法：

1. 遇到 ( ，当前 index i 入栈
2. 遇到 ) 且栈空，更新左指针为当前位置
3. 遇到 ) 且栈非空，pop 栈，如果栈空， result = i - left，如果非空 result = i - stack.peek()

# 代码

{% highlight java %}
public class Solution {
    public int longestValidParentheses(String s) {
        int len = s.length();
        int[] result = new int[len + 1];
        result[0] = 0;
        int stack = 0;
        int max = 0;

        for (int i = 1; i <= len; i++) {
            int index = i - 1;
            if (s.charAt(index) == '(') {
                stack++;
                result[i] = 0;
            } else if (s.charAt(index) == ')' && stack > 0) {
                stack--;
                if (s.charAt(index - 1) == ')') {
                    result[i] = result[i - 1] + 2;
                    result[i] += result[i - result[i]];
                } else if (s.charAt(index - 1) == '(') {
                    result[i] = result[i - 2] + 2;
                }
                max = result[i] > max ? result[i] : max;
            } else {
                result[i] = 0;
            }
        }
        return max;
    }
}
{% endhighlight %}

# discuss

{% highlight java %}
public class Solution {
    public int longestValidParentheses(String s) {
        Stack<Integer> stack = new Stack<Integer>();
        int max=0;
        int left = -1;
        for(int j=0;j<s.length();j++){
            if(s.charAt(j)=='(') stack.push(j);            
            else {
                if (stack.isEmpty()) left=j;
                else{
                    stack.pop();
                    if(stack.isEmpty()) max=Math.max(max,j-left);
                    else max=Math.max(max,j-stack.peek());
                   }
                }
            }
        return max;
    }
}
{% endhighlight %}