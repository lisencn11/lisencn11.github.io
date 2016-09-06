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

# 二刷

{% highlight java %}
public class Solution {
    public double myPow(double x, int n) {
        if (n == 0) return 1;
        if (n == 1) return x;
        long pow = (long) n;
        pow = pow > 0 ? pow : -pow;
        double ret;
        if (pow % 2 == 1) {
            long half = pow / 2;
            double temp = myPow(x, (int) half);
            ret = temp * temp * x;
        } else {
            long half = pow / 2;
            double temp = myPow(x, (int) half);
            ret = temp * temp;
        }
        
        return n > 0 ? ret : 1 / ret;
        
    }
}
{% endhighlight %}