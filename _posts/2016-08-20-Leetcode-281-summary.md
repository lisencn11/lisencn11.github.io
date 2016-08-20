---
layout: post
title: Leetcode Problem 281 Summary
date: 2016-08-20
categories: blog
tags: [study]

---

# 题目

Given two 1d vectors, implement an iterator to return their elements alternately.

For example, given two 1d vectors:

v1 = [1, 2]  
v2 = [3, 4, 5, 6]

By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1, 3, 2, 4, 5, 6].

Follow up: What if you are given k 1d vectors? How well can your code be extended to such cases?

Clarification for the follow up question - Update (2015-09-18):

The "Zigzag" order is not clearly defined and is ambiguous for k > 2 cases. If "Zigzag" does not look right to you, replace "Zigzag" with "Cyclic". For example, given the following input:

[1,2,3]  
[4,5,6,7]  
[8,9]  
It should return [1,4,8,2,5,9,3,6,7].

# 我的算法

根据两个指针的当前位置判断该输出谁

# 代码

{% highlight java %}
public class ZigzagIterator {
    int i1;
    int i2;
    List<Integer> v1;
    List<Integer> v2;

    public ZigzagIterator(List<Integer> v1, List<Integer> v2) {
        this.v1 = v1;
        this.v2 = v2;
        i1 = 0;
        i2 = 0;
    }

    public int next() {
        if (i1 >= v1.size()) return v2.get(i2++);
        if (i2 >= v2.size()) return v1.get(i1++);
        return i1 == i2 ? v1.get(i1++) : v2.get(i2++);
    }

    public boolean hasNext() {
        return i1 < v1.size() || i2 < v2.size();
    }
}

/**
 * Your ZigzagIterator object will be instantiated and called as such:
 * ZigzagIterator i = new ZigzagIterator(v1, v2);
 * while (i.hasNext()) v[f()] = i.next();
 */
{% endhighlight %}

# discuss

对于 Follow up 问题，可以用一个 Queue 存储所有的 Iterator ，对每个 Iterator ，如果还有下一个元素就扔回 Queue。

{% highlight java %}
public class ZigzagIterator {
    LinkedList<Iterator> list;
    public ZigzagIterator(List<Integer> v1, List<Integer> v2) {
        list = new LinkedList<Iterator>();
        if(!v1.isEmpty()) list.add(v1.iterator());
        if(!v2.isEmpty()) list.add(v2.iterator());
    }

    public int next() {
        Iterator poll = list.remove();
        int result = (Integer)poll.next();
        if(poll.hasNext()) list.add(poll);
        return result;
    }

    public boolean hasNext() {
        return !list.isEmpty();
    }
}
{% endhighlight %}