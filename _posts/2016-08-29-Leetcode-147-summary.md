---
layout: post
title: Leetcode Problem 147 Summary
date: 2016-08-29
categories: blog
tags: [study]

---

# 题目

Sort a linked list using insertion sort.

# 我的算法

对单链表模拟插入排序

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
    public ListNode insertionSortList(ListNode head) {
        if (head == null) return head;
        ListNode fakeHead = new ListNode(0);
        fakeHead.next = head;
        ListNode tail = head;
        ListNode p = head.next;
        while (p != null) {
            ListNode search = fakeHead;
            while (p != search.next && p.val >= search.next.val) {
                search = search.next;
            }
            if (p != search.next) {
                tail.next = p.next;
                p.next = search.next;
                search.next = p;
                p = tail.next;
            } else {
                p = p.next;
                tail = tail.next;
            }
        }
        return fakeHead.next;
    }
}
{% endhighlight %}