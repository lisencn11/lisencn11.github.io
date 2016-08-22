---
layout: post
title: Leetcode Problem 319 Summary
date: 2016-08-21
categories: blog
tags: [study]

---

# 题目

There are n bulbs that are initially off. You first turn on all the bulbs. Then, you turn off every second bulb. On the third round, you toggle every third bulb (turning on if it's off or turning off if it's on). For the ith round, you toggle every i bulb. For the nth round, you only toggle the last bulb. Find how many bulbs are on after n rounds.

Example:

Given n = 3. 

At first, the three bulbs are [off, off, off].  
After first round, the three bulbs are [on, on, on].  
After second round, the three bulbs are [on, off, on].  
After third round, the three bulbs are [on, off, off]. 

So you should return 1, because there is only one bulb is on.

# 我的算法

刚开始直接模拟开关过程，发现超时。。。

于是纸上模拟整个过程发现当 n 为平方数时灯泡是开的。

解释在[这里](https://discuss.leetcode.com/topic/31929/math-solution)

简单来说，任何数拆为乘积的情况是都是一对一对的，比如 12 = 1 * 12 = 2 * 6 = 3 * 4，意味着 12 灯泡要转换 6 次，那么一定是关着的，但是平方数例外：4 = 1 * 4 = 2 * 2，也就是说平方数转换奇数次，所以仍然是开着的。

# 代码

{% highlight java %}
public class Solution {
    public int bulbSwitch(int n) {
        int count = 0;
        for (int i = 1; i * i <= n; i++) count++;
        return count;
    }
}
{% endhighlight %}