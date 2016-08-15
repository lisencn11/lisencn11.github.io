---
layout: post
title: Leetcode Problem 13 Summary
date: 2016-08-14
categories: blog
tags: [study]

---

# 题目

Given a linked list, swap every two adjacent nodes and return its head.

For example,
Given 1->2->3->4, you should return the list as 2->1->4->3.

Your algorithm should use only constant space. You may not modify the values in the list, only nodes itself can be changed.

# 我的算法

这个操作需要三个节点 pre iter iterNext 画图明白操作顺序后就比较简单了。

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
    public ListNode swapPairs(ListNode head) {
        ListNode fakeHead = new ListNode(0);
        fakeHead.next = head;
        ListNode iter = head;
        ListNode pre = fakeHead;
        ListNode iterNext = null;
        
        while (iter != null && iter.next != null) {
            iterNext = iter.next;
            pre.next = iterNext;
            iter.next = iterNext.next;
            iterNext.next = iter;
            pre = iter;
            iter = iter.next;
        }
        
        return fakeHead.next;
    }
}
{% endhighlight %}