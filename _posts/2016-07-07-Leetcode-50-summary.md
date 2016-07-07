---
layout: post
title: Leetcode Problem 50 Summary
date: 2016-07-07
categories: blog
tags: [study]

---

# 题目

实现pow(x, n)

# 我的算法

二分查找，如果n是奇数，返回pow(x, n / 2) * pow(x, n / 2) * x，偶数的话不需要最后的x。主要是注意corner case：n为负怎么办？n为-2147483648，即MIN_VALUE怎么办，此时能否简单取负？等等。

# 代码

{% highlight java %}
public class Solution {
    Map<Long, Double> map = null;
    
    public double myPow(double x, int n) {
        long longN = n;
        if (longN < 0) {
            x = 1 / x;
            longN = -longN;
        }
        map = new HashMap<Long, Double>();
        return powHelper(x, longN);
    }
    
    private double powHelper(double x, long n) {
        if (n == 1) {
            return x;
        }
        if (n == 0) {
            return 1;
        }
        if (map.containsKey(new Long(n))) {
            return map.get(new Long(n));
        }
        double result;
        if (n % 2 == 1) {
            result = powHelper(x, n / 2) * powHelper(x, n / 2) * x;
        } else {
            result = powHelper(x, n / 2) * powHelper(x, n / 2);
        }
        map.put(n, result);
        return result;
    }
}
{% endhighlight %}

# 改进

该算法复杂度无需改进，但是其中的逻辑判断略显复杂，在面试过程中很容易出错，故对代码进行改进。

* 如果汽车从A能开到B-1，但不能开到B，则从A和B间任何一点都不能开到B。
* 如果所有加油站的累积邮箱改变量为正，则一定存在一个加油站能作为起点。

{% highlight java %}
public class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int start = 0, total = 0, tank = 0;
        
        for (int i = 0; i < gas.length; i++) {
            if ((tank += gas[i] - cost[i]) < 0) {
                start = i + 1;
                total += tank;
                tank = 0;
            }
        }
        
        return (total + tank < 0) ? -1 : start;
    }
}
{% endhighlight %}