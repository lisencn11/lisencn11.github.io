---
layout: post
title: Leetcode Problem 295 Summary
date: 2016-09-02
categories: blog
tags: [study]

---

# 题目

Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.

Examples:   
[2,3,4] , the median is 3

[2,3], the median is (2 + 3) / 2 = 2.5

Design a data structure that supports the following two operations:

* void addNum(int num) - Add a integer number from the data stream to the data structure.
* double findMedian() - Return the median of all elements so far.
* 
For example:

add(1)  
add(2)  
findMedian() -> 1.5  
add(3)   
findMedian() -> 2

# 我的算法

用两个 heap ，一个是最小堆，保存右半边的元素，一个是最大堆，保存左半边的元素。

# 代码

{% highlight java %}
public class MedianFinder {
    Queue<Long> large = new PriorityQueue<>();
    Queue<Long> small = new PriorityQueue<>();
    
    // Adds a number into the data structure.
    public void addNum(int num) {
        large.offer((long) num);
        small.offer(-large.poll());
        while (large.size() < small.size()) {
            large.offer(-small.poll());
        }
    }

    // Returns the median of current data stream
    public double findMedian() {
        if (small.size() < large.size()) {
            return (double) large.peek();
        } else {
            return ((double) large.peek() - (double) small.peek()) / 2;
        }
    }
};

// Your MedianFinder object will be instantiated and called as such:
// MedianFinder mf = new MedianFinder();
// mf.addNum(1);
// mf.findMedian();
{% endhighlight %}