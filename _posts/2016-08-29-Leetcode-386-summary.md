---
layout: post
title: Leetcode Problem 386 Summary
date: 2016-08-29
categories: blog
tags: [study]

---

# 题目

Given an integer n, return 1 - n in lexicographical order.

For example, given 13, return:   [1,10,11,12,13,2,3,4,5,6,7,8,9].

Please optimize your algorithm to use less time and space. The input size may be as large as 5,000,000.

# 我的算法

我的解法是实现对三个状态的数进行转化：

1. 乘 10 后仍小于 n 则乘 10
2. 不能乘 10 判断是否最后一位是 9 或者 到达 n，没到的话加 1 就好
3. 如果是末尾到了 9 或者到了 n ，进行去 9 直到保留 1 个，然后去除最后一位加 1 ，之所以要留一位是简化操作，让 n 和末尾 9 进行相同处理。

# 代码

{% highlight java %}
public class Solution {
    public List<Integer> lexicalOrder(int n) {
        List<Integer> list = new ArrayList<>();
        int curr = 1;
        for (int i = 1; i <= n; i++) {
            list.add(curr);
            if (curr * 10 <= n) {
                curr *= 10;
            } else if (curr % 10 != 9 && curr + 1 <= n) {
                curr++;
            } else {
                while ((curr / 10) % 10 == 9) {
                    curr /= 10;
                }
                curr = curr / 10 + 1;
            }
        }
        return list;
    }
}
{% endhighlight %}