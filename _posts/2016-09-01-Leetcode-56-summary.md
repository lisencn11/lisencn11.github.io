---
layout: post
title: Leetcode Problem 56 Summary
date: 2016-09-01
categories: blog
tags: [study]

---

# 题目

Given a collection of intervals, merge all overlapping intervals.

For example,  
Given [1,3],[2,6],[8,10],[15,18],  
return [1,6],[8,10],[15,18].

# 我的算法

按 start time 排序，如果能够和后面的 merge 就 merge。

# 代码

{% highlight java %}
/**
 * Definition for an interval.
 * public class Interval {
 *     int start;
 *     int end;
 *     Interval() { start = 0; end = 0; }
 *     Interval(int s, int e) { start = s; end = e; }
 * }
 */
public class Solution {
    public List<Interval> merge(List<Interval> intervals) {
        Comparator<Interval> comp = new Comparator<Interval>() {
            public int compare(Interval i1, Interval i2) {
                return i1.start - i2.start;
            }
        };
        
        Collections.sort(intervals, comp);
        Interval pre = null;
        List<Interval> ret = new ArrayList<>();
        for (Interval i : intervals) {
            if (pre == null) {
                pre = i;
                continue;
            }
            if (pre.end >= i.start) {
                pre = new Interval(pre.start, Math.max(pre.end, i.end));
            } else {
                ret.add(pre);
                pre = i;
            }
        }
        
        if (pre != null) ret.add(pre);
        return ret;
    }
}
{% endhighlight %}