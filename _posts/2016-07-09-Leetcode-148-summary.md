---
layout: post
title: Leetcode Problem 148 Summary
date: 2016-07-09
categories: blog
tags: [study]

---

# 题目

Sort a linked list in O(n log n) time using constant space complexity.

# 我的算法

归并排序应用于链表上。这里给出自顶向下的归并排序，严格说的话我们这里仍然用了O(logn)的空间复杂度在栈空间上，可以通过改写为自底向上的归并排序。

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
    public ListNode sortList(ListNode head) {
        if (head == null || head.next == null) return head;
        
        ListNode pre = null;
        ListNode p = head;
        ListNode f = head;
        while (p != null && p.next != null) {
            pre = f;
            f = f.next;
            p = p.next.next;
        }
        
        pre.next = null;
        ListNode h2 = sortList(f);
        ListNode h1 = sortList(head);
        return merge(h1, h2);
    }
    
    private ListNode merge(ListNode h1, ListNode h2) {
        ListNode fakeHead = new ListNode(0);
        ListNode iter = fakeHead;
        while (h1 != null && h2 != null) {
            if (h1.val < h2.val) {
                iter.next = h1;
                h1 = h1.next;
            } else {
                iter.next = h2;
                h2 = h2.next;
            }
            iter = iter.next;
        }
        if (h1 != null) iter.next = h1;
        if (h2 != null) iter.next = h2;
        return fakeHead.next;
    }
}
{% endhighlight %}

# 二刷

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
    public ListNode sortList(ListNode head) {
        int len = 0;
        ListNode iter = head;
        while (iter != null) {
            len++;
            iter = iter.next;
        }
        ListNode fakeHead = null;
        for (int step = 1; step < len; step *= 2) {
            fakeHead = new ListNode(0);
            iter = fakeHead;
            fakeHead.next = head;
            ListNode p1 = head;
            ListNode p2 = head;
            for (int i = 0; i < step && p2 != null; i++) {
                p2 = p2.next;
            }
            while (p2 != null) {
                iter = merge(iter, p1, p2, step);
                p1 = iter.next;
                p2 = iter.next;
                for (int i = 0; i < step && p2 != null; i++) {
                    p2 = p2.next;
                }
            }
            head = fakeHead.next;
        }
        return head;
    }
    
    private ListNode merge(ListNode head, ListNode node1, ListNode node2, int len) {
        int count1 = 0;
        int count2 = 0;
        while (count1 < len || count2 < len) {
            if (count1 < len && (count2 >= len || node2 == null || node1.val <= node2.val)) {
                head.next = node1;
                node1 = node1.next;
                count1++;
            } else {
                if (node2 == null) break;
                head.next = node2;
                node2 = node2.next;
                count2++;
            }
            head = head.next;
        }
        head.next = node2;
        return head;
    }
}
{% endhighlight %}