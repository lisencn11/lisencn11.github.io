---
layout: post
title: Leetcode Problem 362 Summary
date: 2016-08-18
categories: blog
tags: [study]

---

# 题目

Design a hit counter which counts the number of hits received in the past 5 minutes.

Each function accepts a timestamp parameter (in seconds granularity) and you may assume that calls are being made to the system in chronological order (ie, the timestamp is monotonically increasing). You may assume that the earliest timestamp starts at 1.

It is possible that several hits arrive roughly at the same time.

Example:

HitCounter counter = new HitCounter();

// hit at timestamp 1.  
counter.hit(1);

// hit at timestamp 2.  
counter.hit(2);

// hit at timestamp 3.  
counter.hit(3);

// get hits at timestamp 4, should return 3.  
counter.getHits(4);

// hit at timestamp 300.  
counter.hit(300);

// get hits at timestamp 300, should return 4.  
counter.getHits(300);

// get hits at timestamp 301, should return 3.  
counter.getHits(301); 

Follow up:  
What if the number of hits per second could be very large? Does your design scale?

# 我的算法

用一个 List 记录每一个 timestamp ，用一个 HashMap<Integer, Integer> 记录 <timestamp, times> ，times 是从 0 时刻 到 timestamp 的 hit 数。

对于查询我们需要首先从 List 中找到 timestamp 的上下界，上界是最大的符合条件的 timestamp ，下界是最大的不符合条件的 timestamp，然后根据这上下界从 HashMap 中找到两个 hit 数做减法。

# 代码

{% highlight java %}
public class HitCounter {
    List<Integer> list;
    Map<Integer, Integer> map;

    /** Initialize your data structure here. */
    public HitCounter() {
        list = new ArrayList<>();
        map = new HashMap<>();
        list.add(0);
        map.put(0, 0);
    }
    
    /** Record a hit.
        @param timestamp - The current timestamp (in seconds granularity). */
    public void hit(int timestamp) {
        if (timestamp > list.get(list.size() - 1)) list.add(timestamp);
        map.put(timestamp, map.containsKey(timestamp) ? map.get(list.get(list.size() - 1)) + 1 : map.get(list.get(list.size() - 2)) + 1);
    }
    
    /** Return the number of hits in the past 5 minutes.
        @param timestamp - The current timestamp (in seconds granularity). */
    public int getHits(int timestamp) {
        int timeBound = timestamp - 299;
        int iter = list.size() - 1;
        int high = 0;
        int low = 0;
        
        while (iter >= 0 && timestamp < list.get(iter)) iter--;
        high = iter;
        
        while (iter >= 0 && timeBound <= list.get(iter)) iter--;
        low = iter;
        
        int highTimes = high >= 0 ? map.get(list.get(high)) : 0;
        int lowTimes = low >= 0 ? map.get(list.get(low)) : 0;
        
        return highTimes - lowTimes;
    }
}

/**
 * Your HitCounter object will be instantiated and called as such:
 * HitCounter obj = new HitCounter();
 * obj.hit(timestamp);
 * int param_2 = obj.getHits(timestamp);
 */
{% endhighlight %}

# discuss

感觉上面的做法不好，因为题目中给了 timestamp 是单调递增的，所以并不需要无限的保存 List 和 HashMap。

{% highlight java %}
public class HitCounter {
    private int[] times;
    private int[] hits;
    /** Initialize your data structure here. */
    public HitCounter() {
        times = new int[300];
        hits = new int[300];
    }
    
    /** Record a hit.
        @param timestamp - The current timestamp (in seconds granularity). */
    public void hit(int timestamp) {
        int index = timestamp % 300;
        if (times[index] != timestamp) {
            times[index] = timestamp;
            hits[index] = 1;
        } else {
            hits[index]++;
        }
    }
    
    /** Return the number of hits in the past 5 minutes.
        @param timestamp - The current timestamp (in seconds granularity). */
    public int getHits(int timestamp) {
        int total = 0;
        for (int i = 0; i < 300; i++) {
            if (timestamp - times[i] < 300) {
                total += hits[i];
            }
        }
        return total;
    }
}
{% endhighlight %}