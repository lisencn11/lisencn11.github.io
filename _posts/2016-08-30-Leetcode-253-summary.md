---
layout: post
title: Leetcode Problem 253 Summary
date: 2016-08-30
categories: blog
tags: [study]

---

# 题目

Given an array of meeting time intervals consisting of start and end times [[s1,e1],[s2,e2],...] (si < ei), find the minimum number of conference rooms required.

For example,  
Given [[0, 30],[5, 10],[15, 20]],  
return 2.

# 我的算法

对 start time 进行排序，然后贪婪算法。

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
    public int minMeetingRooms(Interval[] intervals) {
        Comparator<Interval> comp = new Comparator<Interval>() {
            public int compare(Interval i1, Interval i2) {
                return i1.start - i2.start;
            }
        };
        Arrays.sort(intervals, comp);
        
        List<Integer> list = new ArrayList<>();
        for (Interval i : intervals) {
            boolean arrange = false;
            for (int j = 0; j < list.size(); j++) {
                int end = list.get(j);
                if (end <= i.start) {
                    list.set(j, i.end);
                    arrange = true;
                    break;
                }
            }
            if (!arrange) list.add(i.end);
        }
        return list.size();
    }
}
{% endhighlight %}

# discuss

我用的是 List 遍历找当前最小 end ，但是 discuss 中用了 min heap 很巧妙。