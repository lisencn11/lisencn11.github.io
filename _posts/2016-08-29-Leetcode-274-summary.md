---
layout: post
title: Leetcode Problem 274 Summary
date: 2016-08-29
categories: blog
tags: [study]

---

# 题目

Given an array of citations (each citation is a non-negative integer) of a researcher, write a function to compute the researcher's h-index.

According to the definition of h-index on Wikipedia: "A scientist has index h if h of his/her N papers have at least h citations each, and the other N − h papers have no more than h citations each."

For example, given citations = [3, 0, 6, 1, 5], which means the researcher has 5 papers in total and each of them had received 3, 0, 6, 1, 5 citations respectively. Since the researcher has 3 papers with at least 3 citations each and the remaining two with no more than 3 citations each, his h-index is 3.

Note: If there are several possible values for h, the maximum one is taken as the h-index.

Hint:

1. An easy approach is to sort the array first.
2. What are the possible values of h-index?
3. A faster approach is to use extra space.

# 我的算法

桶排序很好想，这道题我觉得难点在于返回值的选择。从后向前遍历随着 number 的增加和 citation 的减小，我们应该返回什么，以 **有3篇citation至少为6** 和 **有7篇citation至少为5**，那么我们应该返回 5 ，也就是当 number >= citation 时的citation。

# 代码

{% highlight java %}
public class Solution {
    public int hIndex(int[] citations) {
        if (citations == null || citations.length == 0) return 0;
        int maxCite = Integer.MIN_VALUE;
        for (int i = 0; i < citations.length; i++) 
            maxCite = citations[i] > maxCite ? citations[i] : maxCite;
        
        int[] cites = new int[maxCite + 1];
        for (int i = 0; i < citations.length; i++) 
            cites[citations[i]]++;
        
        int number = 0;
        for (int i = cites.length - 1; i >= 0; i--) {
            number += cites[i];
            if (number >= i) return Math.min(number, i); 
        }
        return number;
    }
}
{% endhighlight %}