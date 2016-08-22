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

{% highlight %}
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
{% endhighlight %}

# 二刷更新

第一眼看这道题没思路很正常，我们先尝试能否找到规律。

2 = 1 + 1  
3 = 1 + 2  
4 = 2 + 2  
5 = 2 + 3  
6 = 3 + 3 = 2 + 2 + 2  
3 * 3 > 2 * 2 * 2 

我们发现规律，对于 2 和 3 ，拆分后的乘积不如拆分前，而之后的各数拆分后乘积均大于原数，说明除了 2 和 3 都应该拆分。

又发现 3 * 3 大于 2 * 2 * 2 得出应该尽量拆分成 3 ，而对于 4 而言则应该拆分成 2。

{% highlight java %}
public class Solution {
    public int integerBreak(int n) {
        if (n == 2) return 1;
        if (n == 3) return 2;
        int product = 1;
        int sum = 0;
        
        while (sum < n) {
            if (sum + 2 == n || sum + 4 == n) {
                product *= 2;
                sum += 2;
            } else {
                product *= 3;
                sum += 3;
            }
        }
        
        return product;
    }
}
{% endhighlight %}