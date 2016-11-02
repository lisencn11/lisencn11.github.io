---
layout: post
title: Leetcode Problem 413 Summary
date: 2016-10-15
categories: blog
tags: [study]

---

# 题目

Write a program that outputs the string representation of numbers from 1 to n.

But for multiples of three it should output “Fizz” instead of the number and for the multiples of five output “Buzz”. For numbers which are multiples of both three and five output “FizzBuzz”.

Example:

n = 15,

Return:

[  
    "1",  
    "2",  
    "Fizz",  
    "4",  
    "Buzz",  
    "Fizz",  
    "7",  
    "8",  
    "Fizz",  
    "Buzz",  
    "11",  
    "Fizz",  
    "13",  
    "14",  
    "FizzBuzz"  
]

# 我的算法

没有算法。

# 代码

{% highlight java %}
public class Solution {
    public List<String> fizzBuzz(int n) {
        ArrayList<String> result = new ArrayList<>();
        
        for (int i = 1; i <= n; i++) {
            if (i % 3 == 0 && i % 5 == 0) {
                result.add("FizzBuzz");
            } else if (i % 3 == 0) {
                result.add("Fizz");
            } else if (i % 5 == 0) {
                result.add("Buzz");
            } else {
                result.add(String.valueOf(i));
            }
        }
        
        return result;
    }
}
{% endhighlight %}