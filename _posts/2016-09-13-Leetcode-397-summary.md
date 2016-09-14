---
layout: post
title: Leetcode Problem 397 Summary
date: 2016-09-13
categories: blog
tags: [study]

---

# 题目

Given a positive integer n and you can do operations as follow:

1. If n is even, replace n with n/2.
2. If n is odd, you can replace n with either n + 1 or n - 1.
3. 
What is the minimum number of replacements needed for n to become 1?

Example 1:

Input:  
8

Output:  
3

Explanation:  
8 -> 4 -> 2 -> 1

Example 2:

Input:  
7

Output:  
4

Explanation:  
7 -> 8 -> 4 -> 2 -> 1  
or  
7 -> 6 -> 3 -> 2 -> 1

# 我的算法

我没做出来，详细解释戳[这里](https://discuss.leetcode.com/topic/58334/a-couple-of-java-solutions-with-explanations)。

# 代码

{% highlight java %}
public class Solution {
    public int integerReplacement(int n) {
        int count = 0;
        while (n != 1) {
            if ((n & 1) == 0) {
                n >>>= 1;
            } else if (n == 3 || (Integer.bitCount(n - 1) < Integer.bitCount(n + 1))) {
                n--;
            } else {
                n++;
            }
            count++;
        }
        return count;
    }
}
{% endhighlight %}