---
layout: post
title: Leetcode Problem 252 Summary
date: 2016-08-12
categories: blog
tags: [study]

---

# 题目

Given an array of meeting time intervals consisting of start and end times [[s1,e1],[s2,e2],...] (si < ei), determine if a person could attend all meetings.

For example,  
Given [[0, 30],[5, 10],[15, 20]],  
return false.

# 我的算法

目标是找出区间重叠，对区间排序后比较后一个区间的start和前一个区间的end即可，掌握Comparator写法。

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
    public boolean canAttendMeetings(Interval[] intervals) {
        Comparator<Interval> comp = new Comparator<Interval>() {
            public int compare(Interval o1, Interval o2) {
                return o1.start - o2.start;
            }
        };
        Arrays.sort(intervals, comp);
        
        for (int i = 0; i < intervals.length - 1; i++)
            if (intervals[i].end > intervals[i + 1].start) return false;
        return true;
    }
}
{% endhighlight %}