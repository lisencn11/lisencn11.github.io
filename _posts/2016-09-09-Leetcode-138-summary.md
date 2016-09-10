---
layout: post
title: Leetcode Problem 138 Summary
date: 2016-09-09
categories: blog
tags: [study]

---

# 题目

A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.

# 我的算法

链表结点中有一个随机结点域，需要用 Hash Table 先将原结点和拷贝结点对应存储下来，后面可能会用。

# 代码

{% highlight java %}
/**
 * Definition for singly-linked list with a random pointer.
 * class RandomListNode {
 *     int label;
 *     RandomListNode next, random;
 *     RandomListNode(int x) { this.label = x; }
 * };
 */
public class Solution {
    public RandomListNode copyRandomList(RandomListNode head) {
        Map<RandomListNode, RandomListNode> map = new HashMap<>();
        RandomListNode iter = head;
        RandomListNode fakeHead = new RandomListNode(0);
        RandomListNode copyIter = fakeHead;
        while (iter != null) {
            RandomListNode copy = null;
            if (map.containsKey(iter)) copy = map.get(iter);
            else {
                copy = new RandomListNode(iter.label);
                map.put(iter, copy);
            }
            if (map.containsKey(iter.random)) copy.random = map.get(iter.random);
            else {
                if (iter.random == null) copy.random = null;
                else {
                    copy.random = new RandomListNode(iter.random.label);
                    map.put(iter.random, copy.random);
                }
            }
            copyIter.next = copy;
            copyIter = copy;
            iter = iter.next;
        }
        copyIter.next = null;
        return fakeHead.next;
    }
}
{% endhighlight %}