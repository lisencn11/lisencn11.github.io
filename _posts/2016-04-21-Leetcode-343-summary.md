---
layout: post
title: Leetcode Problem 343 Summary
date: 2016-04-21
categories: blog
tags: [study]

---

# 题目

输入是一个正整数n，要求把n分成加和的形式，即n=n0+n1+..+nk，使得各部分的乘积最小，即n0n1...nk最小。要求至少分成两段，且假设n至少为2。

例：n=2，输出1(2=1+1)。n=10，输出36(10=3+3+4)。

# 算法

这其实是一道找规律的题，多写几个数的乘积最大分割，我们会发现分割成的乘积单元均为2或3。

证明如下：假设对于p的乘积当前分割为p=p1+p2+...+pi+...pk，我们把pi继续分割，并要求保持乘积不小于当前分割，可得2*(pi-2)>=pi，从而得到pi>=4，即在pi>=4的情况下我们对它继续分割可至少保持乘积不小于当前分割。进而我们得出结论，加和分割中不能有大于等于4的分割单元，当然显然等于1的分割单元是一种浪费，所以这个数会被分为2和3的加和。

# 代码

```java

public class Solution {
    public int integerBreak(int n) {
        if (n == 2) {
            return 1;
        } else if (n == 3) {
            return 2;
        } else if ((n % 3) == 0) {
            return (int)Math.pow(3, n/3);
        } else if ((n % 3) == 1) {
            return (int)Math.pow(3, (n-4)/3) * 2 * 2;
        } else {
            return (int)Math.pow(3, n/3) * 2;
        }
    }
}

```