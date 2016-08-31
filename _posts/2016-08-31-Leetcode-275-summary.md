---
layout: post
title: Leetcode Problem 275 Summary
date: 2016-08-31
categories: blog
tags: [study]

---

# 题目

Follow up for H-Index: What if the citations array is sorted in ascending order? Could you optimize your algorithm?

Hint:

Expected runtime complexity is in O(log n) and the input is sorted.

# 我的算法

二分查找，如果有 从后向前 len = citations 就直接返回，否则返回第一个 len > citation 的 len - 1。

# 代码

{% highlight java %}
public class Solution {
    public int hIndex(int[] citations) {
        int len = citations.length;
        int low = 0;
        int high = len - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            int num = len - mid;
            if (citations[mid] > num) high = mid - 1;
            else if (citations[mid] < num) low = mid + 1;
            else return num;
        }
        return len - low;
    }
}
{% endhighlight %}