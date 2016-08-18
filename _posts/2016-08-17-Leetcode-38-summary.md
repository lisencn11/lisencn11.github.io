---
layout: post
title: Leetcode Problem 38 Summary
date: 2016-08-17
categories: blog
tags: [study]

---

# 题目

The count-and-say sequence is the sequence of integers beginning as follows:

1, 11, 21, 1211, 111221, ...

1 is read off as "one 1" or 11.  
11 is read off as "two 1s" or 21.  
21 is read off as "one 2, then one 1" or 1211.

Given an integer n, generate the nth sequence.

Note: The sequence of integers will be represented as a string.

# 我的算法

算法就是先 count 有多少个相同数字，然后输出即可。

# 代码

{% highlight java %}
public class Solution {
    public String countAndSay(int n) {
        StringBuilder num = new StringBuilder("1");
        
        for (int i = 1; i < n; i++) {
            StringBuilder currNum = new StringBuilder();
            int counter = 1;
            
            for (int j = 1; j < num.length(); j++) {
                if (num.charAt(j) == num.charAt(j - 1)) {
                    counter++;
                } else {
                    currNum.append(counter);
                    currNum.append(num.charAt(j - 1));
                    counter = 1;
                }
            }
            currNum.append(counter);
            currNum.append(num.charAt(num.length() - 1));
            num = currNum;
        }
        
        return num.toString();
    }
}
{% endhighlight %}