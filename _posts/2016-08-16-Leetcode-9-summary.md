---
layout: post
title: Leetcode Problem 9 Summary
date: 2016-08-16
categories: blog
tags: [study]

---

# 题目

Determine whether an integer is a palindrome. Do this without extra space.

click to show spoilers.

Some hints:
Could negative integers be palindromes? (ie, -1)

If you are thinking of converting the integer to string, note the restriction of using extra space.

You could also try reversing an integer. However, if you have solved the problem "Reverse Integer", you know that the reversed integer might overflow. How would you handle such case?

There is a more generic way of solving this problem.

# 我的算法

题目要求不能使用额外空间，否则我会用Stack来做，check是否palindrome只能通过比较对应位大小，而前半部分无法在O(1)时间获得，提示我可以用一个数记录reverse后的数与原数进行比较。

# 代码

{% highlight java %}
public class Solution {
    public boolean isPalindrome(int x) {
        if (x < 0) return false;
        int palindrome = 0;
        int origin = x;
        
        while (origin != 0) {
            int digit = origin % 10;
            origin /= 10;
            palindrome = palindrome * 10 + digit;
        }
        
        return palindrome == x;
    }
}
{% endhighlight %}