---
layout: post
title: Leetcode Problem 233 Summary
date: 2016-09-24
categories: blog
tags: [study]

---

# 题目

Given an integer n, count the total number of digit 1 appearing in all non-negative integers less than or equal to n.

For example:
Given n = 13,
Return 6, because digit 1 occurred in the following numbers: 1, 10, 11, 12, 13.

Hint:

Beware of overflow.

# 我的算法

[详细解释](https://discuss.leetcode.com/topic/18972/ac-short-java-solution/2)

基本解释是：分别计算每一位的 1 ，对于个分位的 1 ，每10个数中又一个，所以用 n / 1 / 10 后，得到有多少个 10 可求得。十分位、百分位同理。

上述算法对于 1XXX的情况会失效，对于 0XXX 算出来是 0，一个都不会加，对于 2XXX 算出来是全部加上，对于1XXX我们的做法是加上后面的余数 (digit % 10 == 1 ? remain + 1 : 0) 的意思就是这个。(digit + 8) 的目的是筛选出 1 和 0 除 10 为 0，保证不会加上当前位的 1。

# 代码

{% highlight java %}
public class Solution {
    public int countDigitOne(int n) {
        int count = 0;
        for (long i = 1; i <= n; i *= 10) {
            long digit = n / i, remain = n % i;
            count += (digit + 8) / 10 * i + (digit % 10 == 1 ? remain + 1 : 0);
        }
        return count;
    }
}
{% endhighlight %}