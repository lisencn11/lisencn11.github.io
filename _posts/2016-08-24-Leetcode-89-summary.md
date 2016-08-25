---
layout: post
title: Leetcode Problem 89 Summary
date: 2016-08-24
categories: blog
tags: [study]

---

# 题目

The gray code is a binary numeral system where two successive values differ in only one bit.

Given a non-negative integer n representing the total number of bits in the code, print the sequence of gray code. A gray code sequence must begin with 0.

For example, given n = 2, return [0,1,3,2]. Its gray code sequence is:

00 - 0  
01 - 1  
11 - 3  
10 - 2

Note:  
For a given n, a gray code sequence is not uniquely defined.

For example, [0,2,3,1] is also a valid gray code sequence according to the above definition.

For now, the judge is able to judge based on one instance of gray code sequence. Sorry about that.

# 我的算法

我发现对于第 n 位为 1 的格雷码，我们只需要先得到第 n - 1 位为 1 的格雷码，将其第 n 位为 1 后倒序输出即可保证连续两数只有一位不同。

# 代码

{% highlight java %}
public class Solution {
    public List<Integer> grayCode(int n) {
        if (n == 0) {
            List<Integer> list = new ArrayList<>();
            list.add(0);
            return list;
        }
        
        List<Integer> list = grayCode(n - 1);
        for (int i = list.size() - 1; i >= 0; i--) {
            int num = list.get(i) | (1 << (n - 1));
            list.add(num);
        }
        
        return list;
    }
}
{% endhighlight %}

# discuss

格雷码有公式 G(i) = i ^ (i / 2) i 表示第 i 个格雷码。

{% highlight java %}
public List<Integer> grayCode(int n) {
    List<Integer> result = new LinkedList<>();
    for (int i = 0; i < 1<<n; i++) result.add(i ^ i>>1);
    return result;
}
{% endhighlight %}