---
layout: post
title: Leetcode Problem 357 Summary
date: 2016-06-22
categories: blog
tags: [study]

---

# 题目

**输入**一个非负整数n。

**输出**从0 <= x < 10<sup>n</sup>之间所有满足此条件的数的个数：这个数的每一位都不相同。例：1234567

输入：2;

输出：2 排除[11, 22, 33, 44, 55...]

# 我的算法

动态规划思路：当n为0，返回1。当n为1，返回9 + 1。此时发现存在递推关系：假设我们已知n=i-1时，即i-1位数组成的每位唯一数，求n=i我们只需要对这些数最后一位加一个数即可，最后一位有(10 - (i - 1))种可能，故为a<sub>n</sub> = a<sub>n-1</sub> * (10 - (i - 1))。最后将a<sub>n</sub>加和即可。

# 代码

{% highlight java %}
public class Solution {
    public int countNumbersWithUniqueDigits(int n) {
        if (n == 0) {
            return 1;
        }
        int[] totalCount = new int[10];
        int[] nthCount = new int[10];
        totalCount[0] = 10;
        nthCount[0] = 9;
        
        if (1 == n) {
            return totalCount[0];
        }
        
        for (int i = 1, j = 9; i < 10; i++, j--) {
            nthCount[i] = nthCount[i - 1] * j;
            totalCount[i] = totalCount[i - 1] + nthCount[i];
            if ((i + 1) == n) {
                return totalCount[i];
            }
        }
        
        if (n > 10) {
            return totalCount[9];
        }
        return -1;
    }
}
{% endhighlight %}

# 二刷update

当时做这题脑子不清醒啊。。。

很简单的就是对于 n 位数，第一位有 9 种可能（排除 0 ），后面各位依次有 9 8 7 6 ... 1 种可能，相乘即可。

### 代码

{% highlight java %}
public class Solution {
    public int countNumbersWithUniqueDigits(int n) {
        if (n == 0) return 1;
        
        int preCount = countNumbersWithUniqueDigits(n - 1);
        int count = 9;
        int base = 9;
        for (int i = 2; i <= n; i++) {
            count *= base--;
            if (base == 0) break;
        }
        
        return count + preCount;
    }
}
{% endhighlight %}