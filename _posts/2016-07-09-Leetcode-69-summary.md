---
layout: post
title: Leetcode Problem 69 Summary
date: 2016-07-09
categories: blog
tags: [study]

---

# 题目

**输入**一个整型。

**输出**这个整型的平方根。

# 我的算法

从0到输入的数，二分查找，需要注意利用mid * mid判断的时候，mid要声明为long型，否则存在乘积溢出的可能。

# 代码

{% highlight java %}
public class Solution {
    public int mySqrt(int x) {
        long low = 0;
        long high = (long)x;
        
        while (low <= high) {
            long mid = low + (high - low) / 2;
            if (mid * mid > x) {
                high = mid - 1;
            } else if (mid * mid < x) {
                low = mid + 1;
            } else {
                return (int)mid;
            }
        }
        
        return (int)high;
    }
}
{% endhighlight %}