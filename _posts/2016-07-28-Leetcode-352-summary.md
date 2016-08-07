---
layout: post
title: Leetcode Problem 352 Summary
date: 2016-07-28
categories: blog
tags: [study]

---

# 题目

有一个数据流，里面都是非负整型，读入数据流，统计不相交的连续数据段。比如一个数据是 1，3，7，2，6 ：

产生的数据段依次为

[1, 1]  
[1, 1], [3, 3]  
[1, 1], [3, 3], [7, 7]  
[1, 3], [7, 7]  
[1, 3], [6, 7]  

# 我的算法

最初的想法是使用优先队列，读入数据流，完成排序，然后用 O(n) 的时间完成对有序数据流的分段。然而发现超时，说明数据结构和算法需要改进。

discuss 中提到的是使用红黑树数据结构的解法，满足题目的时间要求。

之前没有接触过红黑树和TreeMap，总结在[这里]()

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
public class SummaryRanges {
    TreeMap<Integer, Interval> tree;

    /** Initialize your data structure here. */
    public SummaryRanges() {
        tree = new TreeMap<>();
    }
    
    public void addNum(int val) {
        if (tree.containsKey(val)) return;
        Integer l = tree.lowerKey(val);
        Integer h = tree.higherKey(val);
        if ((l != null) && (h != null) && (tree.get(l).end == val - 1) && (h == val + 1)) {
            tree.get(l).end = tree.get(h).end;
            tree.remove(h);
        } else if ((l != null) && (tree.get(l).end + 1 >= val)) {
            tree.get(l).end = Math.max(tree.get(l).end, val);
        } else if ((h != null) && (h == val + 1)) {
            tree.put(val, new Interval(val, tree.get(h).end));
            tree.remove(h);
        } else {
            tree.put(val, new Interval(val, val));
        }
    }
    
    public List<Interval> getIntervals() {
        return new ArrayList<>(tree.values());
    }
}

/**
 * Your SummaryRanges object will be instantiated and called as such:
 * SummaryRanges obj = new SummaryRanges();
 * obj.addNum(val);
 * List<Interval> param_2 = obj.getIntervals();
 */
 {% endhighlight %}