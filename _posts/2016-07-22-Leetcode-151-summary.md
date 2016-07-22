---
layout: post
title: Leetcode Problem 151 Summary
date: 2016-07-22
categories: blog
tags: [study]

---

# 题目

**输入**一个 String 字符串。

**输出**将字符串中的各单词倒序，但是单词内字符不倒序。

需要确定的细节：

1. 原字符串可以包含头空格和尾空格，单词间可以有多个空格。
2. 倒序后头空格和尾空格需要消除，单词间也只允许一个空格。

# 我的算法

使用 String 的 split() 方法和 StringBuilder 类即可，注意边界情况：纯空格字符串。

# 代码

{% highlight java %}
public class Solution {
    public String reverseWords(String s) {
        if (s == null || s.length() == 0) return s;
        
        String[] str = s.split(" ");
        StringBuilder sb = new StringBuilder();
        for (int i = str.length - 1; i >= 0; i--)
            if (str[i].length() != 0) sb.append(str[i] + " ");
        if (sb.length() > 0) sb.delete(sb.length() - 1, sb.length());
        
        return sb.toString();
    }
}
{% endhighlight %}