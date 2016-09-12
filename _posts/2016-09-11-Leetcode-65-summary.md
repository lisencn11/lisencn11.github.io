---
layout: post
title: Leetcode Problem 65 Summary
date: 2016-09-11
categories: blog
tags: [study]

---

# 题目

Validate if a given string is numeric.

Some examples:  
"0" => true  
" 0.1 " => true  
"abc" => false  
"1 a" => false  
"2e10" => true

Note: It is intended for the problem statement to be ambiguous. You should gather all requirements up front before implementing one.

# 我的算法

一个数字的合法字符有：

1. 0-9 数字
2. . 小数点
3. e 指数符号
4. + - 正负号

我们有下面限制条件：

1. 如果我们看见了 . ，我们 check 之前是否见过 e 或 . ，如果见过则返回 false
2. 如果我们看见了 e ，我们 check 之前是否见过 e 或 number ，如果见过 e 或者没见过 number 则返回 false
3. 如果遇到 + - 号，我们 check 是否是第一个位置，或者前面是否是 e ，如果都不是返回 false
4. 如果遇到其他符号返回 false
5. 最后判断是否遇见过 number ，是否同时遇到过 e 和 e 后面的 number

# 代码

{% highlight java %}
public class Solution {
    public boolean isNumber(String s) {
        s = s.trim();
        boolean pointSeen = false;
        boolean eSeen = false;
        boolean numSeen = false;
        boolean eNumSeen = false;
        
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) <= '9' && s.charAt(i) >= '0') {
                numSeen = true;
                eNumSeen = true;
            } else if (s.charAt(i) == '.') {
                if (eSeen || pointSeen) {
                    return false;
                }
                pointSeen = true;
            } else if (s.charAt(i) == 'e') {
                if (!numSeen || eSeen) {
                    return false;
                }
                eSeen = true;
                eNumSeen = false;
            } else if (s.charAt(i) == '+' || s.charAt(i) == '-') {
                if (i != 0 && s.charAt(i - 1) != 'e') {
                    return false;
                }
            } else {
                return false;
            }
        }
        return numSeen && eNumSeen;
    }
}
{% endhighlight %}