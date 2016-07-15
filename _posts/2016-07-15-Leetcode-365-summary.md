---
layout: post
title: Leetcode Problem 365 Summary
date: 2016-07-15
categories: blog
tags: [study]

---

# 题目

**输入**有两个桶，一个能装 x 升，一个能装 y 升，输入一个指定的z升。

**输出**用这一个或两个桶能否装出z升水。

允许的操作：

* 向桶中加满水
* 从桶中倒出所有水
* 从一个桶 a 中向另一个桶 b 中倒水，直到 a 空或 b 满

# 我的算法

没有想出算法，讨论区给出用**线性同余方程**来做，但是还没看懂。。。待看懂后贴出我的理解。

# 代码

{% highlight java %}
public class Solution {
    public boolean canMeasureWater(int x, int y, int z) {
        return z == 0 || (long)x + y >= z && z % gcd(x, y) == 0;
    }
    public int gcd(int x, int y) {
        return y == 0 ? x : gcd(y, x % y);
    }
}
{% endhighlight %}