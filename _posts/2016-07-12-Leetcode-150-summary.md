---
layout: post
title: Leetcode Problem 150 Summary
date: 2016-07-12
categories: blog
tags: [study]

---

# 题目

**输入**String数组存储的逆波兰表达式。

**输出**表达式运算结果。

如：  
>["2", "1", "+", "3", "*"] -> ((2 + 1) * 3) -> 9  
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