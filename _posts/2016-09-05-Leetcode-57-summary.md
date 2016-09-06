---
layout: post
title: Leetcode Problem 305 Summary
date: 2016-09-05
categories: blog
tags: [study]

---

# 题目

Given a set of non-overlapping intervals, insert a new interval into the intervals (merge if necessary).

You may assume that the intervals were initially sorted according to their start times.

Example 1:  
Given intervals [1,3],[6,9], insert and merge [2,5] in as [1,5],[6,9].

Example 2:  
Given [1,2],[3,5],[6,7],[8,10],[12,16], insert and merge [4,9] in as [1,2],[3,10],[12,16].

This is because the new interval [4,9] overlaps with [3,5],[6,7],[8,10].

# 我的算法

分为三部分：

1. 前部无重叠部分
2. 中部有重叠部分
3. 后部无重叠部分

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
    public List<Interval> insert(List<Interval> intervals, Interval newInterval) {
        List<Interval> result = new ArrayList<>();
        int index = 0;
        int len = intervals.size();
        
        while (index < len && intervals.get(index).end < newInterval.start) {
            result.add(intervals.get(index++));
        }
        
        while (index < len && intervals.get(index).start <= newInterval.end) {
            newInterval.start = Math.min(intervals.get(index).start, newInterval.start);
            newInterval.end = Math.max(intervals.get(index++).end, newInterval.end);
        }
        result.add(newInterval);
        
        while (index < len) {
            result.add(intervals.get(index++));
        }
        return result;
    }
}
{% endhighlight %}