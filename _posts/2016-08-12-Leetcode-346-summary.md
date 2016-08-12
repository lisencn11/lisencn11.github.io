---
layout: post
title: Leetcode Problem 346 Summary
date: 2016-08-12
categories: blog
tags: [study]

---

# 题目

Given a stream of integers and a window size, calculate the moving average of all integers in the sliding window.

For example,  
MovingAverage m = new MovingAverage(3);  
m.next(1) = 1  
m.next(10) = (1 + 10) / 2  
m.next(3) = (1 + 10 + 3) / 3  
m.next(5) = (10 + 3 + 5) / 3

# 我的算法

每当加入一个新的数时，计算出加和的变化量，即新减旧即可。

# 代码

{% highlight java %}
public class MovingAverage {
    int windowSize = 0;
    int currSize = 0;
    double average = 0;
    Queue<Integer> queue = null;

    /** Initialize your data structure here. */
    public MovingAverage(int size) {
        queue = new LinkedList<>();
        windowSize = size;
    }
    
    public double next(int val) {
        queue.offer(val);
        if (currSize < windowSize) {
            average = (average * currSize + val) / (currSize + 1);
            currSize++;
            return average;
        } else {
            double diff = val - queue.poll();
            average += diff / windowSize;
            return average;
        }
    }
}

/**
 * Your MovingAverage object will be instantiated and called as such:
 * MovingAverage obj = new MovingAverage(size);
 * double param_1 = obj.next(val);
 */
{% endhighlight %}