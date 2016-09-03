---
layout: post
title: Leetcode Problem 23 Summary
date: 2016-09-02
categories: blog
tags: [study]

---

# 题目

Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

# 我的算法

用一个 minHeap 时刻追踪每个 linkedlist 的头结点。

# 代码

{% highlight java %}
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        ListNode fakeHead = new ListNode(0);
        ListNode pre = fakeHead;
        Queue<ListNode> minHeap = new PriorityQueue<>(new Comparator<ListNode>() {
            public int compare(ListNode n1, ListNode n2) {
                return n1.val - n2.val;
            }
        });
        
        for (ListNode node : lists) {
            if (node != null) minHeap.offer(node);
        }
        
        while (!minHeap.isEmpty()) {
            ListNode curr = minHeap.poll();
            pre.next = curr;
            pre = curr;
            if (curr.next != null) minHeap.offer(curr.next);
        }
        
        return fakeHead.next;
    }
}
{% endhighlight %}